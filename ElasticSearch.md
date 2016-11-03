#Elastic Search

## WebAPI
 ```sh
http://localhost:9200/_cat/ # List avaialbe operations
http://localhost:9200/_cat/indices?v # Show all indices
http://localhost:9200/test/type/_mget # get multiple documents

http://localhost:9200/inca/employee/_search?q=firstName:nico # search documents with firstName = nico
 ```
 
## Shell

```sh
curl -XDELETE 'localhost:9200/inca?pretty'
```


### Query using DSL
 ```sh

curl -XGET 'localhost:9200/_search?pretty' -d'
{
  "query": { 
    "bool": { 
      "must": [
        { "match": { "title":   "Search"        }}, 
        { "match": { "content": "Elasticsearch" }}  
      ],
      "filter": [ 
        { "term":  { "status": "published" }}, 
        { "range": { "publish_date": { "gte": "2015-01-01" }}} 
      ]
    }
  }
}'
 
 
 
curl -XGET 'localhost:9200/_search?pretty' -d'
{
    "query": {
        "match_all": { "boost" : 1.2 }
    }
}'

 ```
## Field Types 

Exact values (number, date, keyword) => term query
Analyzed (text) => match query
 
 
## Search API

Bool-Query:

* must: must exist and contributes to score
* filter: must exist but does not contribute to score
* should: contributes to score
* must_not: Exclude documents

term: Exact match 
match: First analyzed

###term level queries: 
 
* __term query:__ exact term 
* __terms query:__ any of the exact terms 
* __range query:__  values (dates, numbers, or strings) in the range
* __exists query:__ contains any non-null
* __prefix query:__ contains terms which begin with the exact prefix 
* __wildcard query:__ contains terms which match the pattern specified (single character wildcards (?), multi-character wildcards (*))
* __regexp query:__ contains terms which match the regular expression
* __fuzzy query:__ contains terms which are fuzzily similar (Levenshtein edit distance of 1 or 2)
* __type query:__ documents of the specified type
* __ids query:__ documents with the specified type and IDs

 
## Python without API

### Store multiple elements
```py 
import json
import http.client

conn = http.client.HTTPConnection("localhost", 9200)

with open('employee.json') as data_file:    
    headerInfo = {'content-type': 'application/json' }
    for emp in json.load(data_file):
        singleEmp = json.dumps(emp, sort_keys=True, indent=2)
        conn.request("POST", "/inca/employee?pretty",singleEmp, headerInfo)
        response = conn.getresponse()
```

### Query those Elements
```py 
import json
import http.client

conn = http.client.HTTPConnection("localhost", 9200)

with open('employee.json') as data_file:    
    headerInfo = {'content-type': 'application/json' }
    for emp in json.load(data_file):
        singleEmp = json.dumps(emp, sort_keys=True, indent=2)
        conn.request("GET", "/inca/employee?pretty",singleEmp, headerInfo)
        response = conn.getresponse()
```