---
layout: page
title: Hadoop
excerpt: How to install a new Hadoop
search_omit: false
---

Hadoop incorporates a parallel file system (HDFS) and a computational engine:

* **Hadoop File System (HDFS)**: Distribuited storing system. There is redundancy level (by default 2) across all the nodes. On top of this filesystem you can create for example a database. There are different necessary processes required by HDFS:  
  * Name Node: Process/Table which says in which resource the data is stored. Usually there is also a *Secondary Name Node* as a failsafe mechanism.
  * Data Node: Node where the data is really stored.

* **MapReduce**: It is the computational engine for distributed memory and CPU usage.
  * Job Tracker: Scheduling and management
  * Task Tracker: Process usually executed in the node where the data is stored.

* **YARN**: Improved MapReduce version.
  * Resource Manager: Scheduling and management. It is possible to have more than one to add redundancy. 
  * Node Manager


## Simple environment setup

We are going to create a simple environment with just one node.

### Host configuration

It connects to other nodes by ssh, so we need to create public and private keys to connect without password. You can use the commands ssh-keygen and ssh-copy-id to generate and configure the SSH keys. More information available [here](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)


### Hadoop FS configuration.



It is necessary to configure the default port where the Name Node will listen for commands. By default this port is 9000. 

*./etc/hadoop/core-site.xml*
```
<configuration>
    <property>
       <name>fs.defaultFS</name>
       <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

If we are goint to use only one node then we must override the default dfs replication which is 2. So to define the value one, update the file *hdfs-site.xml*

*./etc/hdfs-site.xml*
```
<configuration>
    <property>
       <name>dfs.replication/name>
       <value>1</value>
    </property>
</configuration>

```
By default it uses /tmp as default directory to store all the data. Update this to make the changes in HFS permanent.


Format hdfs, this creates the drive and format all the data
```
./bin/hdfs namenode -format
```

Start the name node, data node and secondary name node:
```
./sbin/start-dfs.sh
```

All Hadoop process are executed in a JVN, so to check that all services are running just invoque jps:
```
$ jps -l
3009 org.apache.hadoop.hdfs.server.namenode.NameNode
3140 org.apache.hadoop.hdfs.server.datanode.DataNode
3309 org.apache.hadoop.hdfs.server.namenode.SecondaryNameNode
4126 sun.tools.jps.Jps
```


Many linux distributios support hdfs in the kernel.
Also there is a HTTP web service in the namenode where it is possible to browse the cotent of the HDF.
By default it is in the port 50070 or 9000 depending on the hadoop version.



#### File managing

Use hdfs command to excute commands in the localhost 
```
./bin/hdfs dfs -mkdir <remotefolder> [url]
./bin/hdfs dfs -ls [url]
./bin/hdfs dfs -put <localfile> <remotefile> [url]
```
By default it assumes that [url] is *hdfs://localhost:9000*

