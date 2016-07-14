# HIVE SUMMARY


Frequently used:

 - `set hive.cli.print.header=true;` show column names
 - `SET skip.header.line.count = 1;` to skip header line
 - `hive --version` Check hive version
 - `mapreduce.map.memory.mb=4096` and `mapreduce.reduce.memory.mb=4096` Increase Memory

1) Overview

2) Setting Up
- CLI vs. Beeline

3) Data Definition
- database
- tables (internal/external)
- partitions and buckets
- views

4) Data Selection and Scope

- JOINS (inner, left/right, outer)
- MAPJOIN (in-memory, small data size)
- UNION ALL
- Fasten queries using WHERE on partitioned columns

5) Data Manipulation

- LOAD (text-import), INSERT (cross-table-movement) and EXPORT/IMPORT (backup, transformation to other DBS)
- ORDER (one reducer), SORT (at each reducer), DISTRIBUTE and CLUSTER (distribute+sort)
- Functions
  - Collection: SIZE
  - Date: 
  - Conditional: COALESCE, IF, CASE WHEN
- Transactions: ACID for ORC

6) Data Aggregation and Sampling

- GROUP BY and GROUPING SETS (multiple groupings at a time)
- ROLLUP (n+1 levels of aggregation) and CUBE (all permutations [2^n])
- HAVING (WHERE on group)
- **Analytic functions**
  - RANK, DENSE\_RANK, ROW\_NUMBER
  - CUME\_DIST, PERCENT\_RANK
  - NTILE
  - LEAD, LAG
  - FIRST\_VALUE, LAST\_VALUE
- **Windows-clause**: ROWS BETWEEN <start\_expr> AND <end\_expr>
  - UNBOUNDED PRECEDING, CURRENT ROW, N PREDECING or FOLLOWING
- **Sampling**
  - Random: `SELECT * FROM <table> DISTRIBUTE BY RAND() SORT BY RAND() LIMIT <n>`
  - Bucket: `SELECT * FROM <table> TABLESAMPLE (BUCKET <bucket nr> OUT OF <total # buckets> ON RAND()`
  - Block: `SELECT * FROM <table> TABLESAMPLE (<N PERECENT | ByteLength | N ROWS>)`

7) Performance

- Utilities: Explain, Analyse
   - AST (Abstract Syntrax Tree)
   - Stage dependencies
   - Stage Plan: Details on what happens when
- **Data Layout**: Partition, Buckets, Index
- **File Format:** 
	- ORC best for Hive (Hortonworks)
	- Parquett has greater portability (Cloudera)
	- Textfile are used for initialization (then INSERT into other table)
- Compression: LZ4 or Snappy
- Storage Optimization
- SerDe: Changing data on input/output

Table Size | Data Profile | Query Pattern | Recommendation
----|----|----|----|
Small | Hot data | Any | Increase replication factor
Any | Any | Very precise filters | Sort on column most frequently used in precise queries
Large | Any | Joined to another large table | Sort and bucket both tables along the join key
Large | One value >25% of count within high cardinality column | Any | Split the frequent value into a separate skew
Large | Any | Queries tend to have a natural boundary such as date | Partition the data along the natural boundary

8) Extensibility

- UDF, UDAF (Aggregatin), UDTF (Table-Generating)
- Deployment: 
  - Write Java code and compile it as JAR
  - Add JAR to HIVE (`ADD JAR {path}`) 
  - Create a function temporary (`CREATE TEMPORARY FUNCTION {name} AS {package-class-spec};`) or permanently (`CREATE FUNCTION {name} AS {package-class-spec} USING JAR '{hdfs-path}'`)
  - Check function (`SHOW FUNCTION {name};`) and use in HQL

9) Security
 - Authentication
 - Authorization: Legacy (simple), Storage-based (HDFS), SQL (column and row level)
 - Encryption: Not out-of-the-box, custom UDFs required

10) Examples

```sql
CREATE TABLE test (
pstring STRING,
pint INT,
pdouble DOUBLE) 
row format delimited fields terminated by ','
STORED AS TEXTFILE;
```


10) Other Tools:

``` sql 
SELECT count(distinct ws1.ws_order_number) as order_count,
       sum(ws1.ws_ext_ship_cost) as total_shipping_cost,
       sum(ws1.ws_net_profit) as total_net_profit
FROM web_sales ws1
JOIN /*MJ1*/ customer_address ca ON (ws1.ws_ship_addr_sk = ca.ca_address_sk)
JOIN /*MJ2*/ web_site s ON (ws1.ws_web_site_sk = s.web_site_sk)
JOIN /*MJ3*/ date_dim d ON (ws1.ws_ship_date_sk = d.d_date_sk)
LEFT SEMI JOIN /*JOIN4*/ (SELECT ws2.ws_order_number as ws_order_number
                          FROM web_sales ws2 JOIN /*JOIN1*/ web_sales ws3
                          ON (ws2.ws_order_number = ws3.ws_order_number)
                          WHERE ws2.ws_warehouse_sk <> ws3.ws_warehouse_sk) ws_wh1
ON (ws1.ws_order_number = ws_wh1.ws_order_number)
LEFT SEMI JOIN /*JOIN4*/ (SELECT wr_order_number
                          FROM web_returns wr
                          JOIN /*JOIN3*/ (SELECT ws4.ws_order_number as ws_order_number
                                          FROM web_sales ws4 JOIN /*JOIN2*/ web_sales ws5
                                          ON (ws4.ws_order_number = ws5.ws_order_number)
                                          WHERE ws4.ws_warehouse_sk <> ws5.ws_warehouse_sk) ws_wh2
                          ON (wr.wr_order_number = ws_wh2.ws_order_number)) tmp1
ON (ws1.ws_order_number = tmp1.wr_order_number)
WHERE d.d_date >= '2001-05-01' and
      d.d_date <= '2001-06-30' and
      ca.ca_state = 'NC' and
      s.web_company_name = 'pri';
```



11) Own Remarks:

 - get Settings: `set .`
 - Set Variables: 
 - Logs: `/var/hive/` or `/tmp/logs/`