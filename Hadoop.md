#Hadoop Ecosystem

## Zookeeper

`alias zkCli=/etc/lib/zookeeper/bin/zkCli.sh`
`/etc/init.d/zo`

## View Logs

Logs: `/var/log/hadoop-yarn`
Settings: `/etc/hive/conf`



## Setup Hadoop high availability Cluster
1. Install Java

2. Install journals
3. install zookeeper
4. install namenode

	1. For Both Namenodes: create namenode and data dirs
	2. On Active-Namenode: 
	
		```
		start hadoop-hdfs-zkfc
		hdfs namenode -format
		/etc/init.d/hadoop-hdfs-namenode start
		```
	
	3. On Inactive-Namenode: 
	
		```
		hdfs namenode -bootstrapStandby
		su hdfs
		/etc/init.d/hadoop-hdfs-namenode start
		```
	
5. install hdfs
6. install yarn
7. install metastore
8. install hive
9. install 

## HDFS
Nutzerberechtigungen werden im Namenode nachgeschlagen

