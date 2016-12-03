---
layout: page
title: Spark
excerpt: How to install a new Spark
search_omit: false
---

# Spark

Can be executed in a single node. It can use hadoop libraries to connecto to HDFS, but it does not replace hadoop or launches it.

* Architecture
** Driver Program: It is our progam.  Worker node can send data to the Driver Program. The Driver Probran interacts only with the Cluster Progamer.
** Cluster Manager: Manages all the Worker Node. It can be:
*** Spark:       (spark://host.port)
*** Apache Hadoop YARN:         : (yarn://host.port)
*** Apache Mesos:  A distributed systems kernel   (mesos://host.port)
*** Or any other Cluster. Also called master.
** Worker Node: Node which hasve executor and tasks. Also called slave. To receive data receive from the workers spark uses netty http://netty.io/

Interactive shell cosole. It is a Driver Application that allows iteractive.
* sparkR : R
* pyspark : Python
* spark-shell: Scala
* spark-submit: Send work to 

Options
--master MASTER_URL         spark://host:port, mesos://host:port, yarn, or local
** 

When local it launches one Worker.
--master local  : Use only 1 core and 1 CPU
--master local[n] : Uses n cores]
--master local[*] : Uses all the available cores.
It cannot use GPU, conly CPU.
It comsumes 1Gbyte.


#### Default configuration

./conf/spark-defaults.conf.template


### Python

sc: spak context, appears in the console initialized with all the configuration in place ready to work.


[galois@vmfedora24 bin]$ ./pyspark --master local[*]


>>> ficheroDatosRDD = sc.textFile("../LICENSE")


Two RDD types:
* Actions: Return a value.
* Transformations: Returns another RDD transformed.




>>> ficheroDAtosRDD.collect() 
Action that returns the content of the RDD as a list.

>>> ficheroDatosRDD.count()
Action that counts number of elements in the RDD.

>>> lineasConLicenseRDD = ficheroDatosRDD.filter(lambda linea : "License" in linea)
Tranformation that filters the RDD and only returns the entries which are evaluated as true.




>>> lineasConLicenseRDD.foreach(lambda cadena: cadena+"x")





Example:
```
# _*_ coding: utf-8 _*_
import pyspark

#Configuration
conf = pyspark.SparkConf()
#conf.setMaster("local[*]")
#conf.setAppName("Basic examples")
#conf.set("", "")

#Execution context
sc = pyspark.SparkContext(conf = conf)
myFile = "/home/galois/spark-2.0.1-bin-hadoop2.7/LICENSE"

#Load text file
fileRDD = sc.textFile(myFile)

#Count elements in the RDD
lineCount = fileRDD.count()
print "Number of lines in the file: ", lineCount

#To filter RDDs
def inLicense(line):
    return "License" in line

linesWithLicences = fileRDD.filter(inLicense)

print "Number of lines with License: ", linesWithLicences.count()

```







Execute:
$ ~/spark-2.0.1-bin-hadoop2.7/bin/spark-submit --master local[*] basico.py
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties











How to count words:


fileRDD.flatMap(lambda l: l.split(" ")) # Maps all the words to a single list
 .map(lambda word: (word, 1) )          # Maps a list of words into a list of tuples (key,value)
 .reduceByKey(lambda prev,act:prev+act) # Sums all the values with the same key




You can create accumulators from classes, you just have to extend from AccumulatorParam and implement the mandatory methods

class MatrixAccumulator(AccumulatorParam):   
    def zero(self, value):
        AccumulatorParam.zero(self, value)
        
    def addInPlace(self, value1, value2):
        AccumulatorParam.addInPlace(self, value1, value2)


## Create Spark cluster

1. First launch master, then get the URL
2. Launch slaves

The master can connect to the server via SSH

## Manual
###Execute in master:
start-master.sh

### Execute in slave:
start-slave.sh 


## Automatic configuration by SSH
start-on.sh
start-slaves.sh (reads config from ./conf/slaves.template)



By default the scheduler uses a FIFO to places all the running applications.
It is possible to change a use another type changing the *spark.scheduler.mode* variable:
```
conf.set("spark.scheduler.mode", "FAIR")
```

It is possible to define diffent pool types and how many cores can be used and the scheduler type editing the file fairscheduler.xml.
conf.set("spark.scheduler.allocation.file","/etc/spark/fairscheduler.xml")
conf.set("spark.scheduler.pool","production")


### Monitor URLs

http://localhost:8081/ : 
http://localhost:4040/jobs/


## SparkR

$ export SPARK_HOME=/path/to/spark/directory
$ cd $SPARK_HOME/R/lib/SparkR/
$ R -e "devtools::install('.')"


