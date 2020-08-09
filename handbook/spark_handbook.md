# Programming Guides

## [Quick Start](https://spark.apache.org/docs/2.4.6/quick-start.html)

### [Interactive Analysis with the Spark Shell](https://spark.apache.org/docs/2.4.6/quick-start.html#interactive-analysis-with-the-spark-shell)

#### [Basics](https://spark.apache.org/docs/2.4.6/quick-start.html#basics)

```
mkdir -p ~/soft/spark/
tar -zxvf spark-2.4.6-bin-without-hadoop-scala-2.12.tgz -C ~/soft/spark/
export SPARK_HOME=`pwd`
export PATH=$SPARK_HOME/bin:$PATH
export PYTHONPATH=/home/alvin/soft/spark/spark-2.4.6/python:$PYTHONPATH
export PYTHONPATH=$SPARK_HOME/python/lib/py4j-0.10.7-src.zip:$PYTHONPATH
export PYSPARK_PYTHON=/usr/bin/python3
```


```
vi ~/.bashrc
```

```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_HOME=/home/alvin/soft/hadoop/hadoop_2_10_0
export SPARK_HOME=/home/alvin/soft/spark/spark-2.4.6
export PYTHONPATH=/home/alvin/soft/spark/spark-2.4.6/python:$PYTHONPATH
export PYTHONPATH=$SPARK_HOME/python/lib/py4j-0.10.7-src.zip:$PYTHONPATH
export PYSPARK_PYTHON=/usr/bin/python3
export PATH=$HADOOP_HOME/bin:$PATH
export PATH=$HADOOP_HOME/sbin:$PATH
export PATH=$SPARK_HOME/bin:$PATH

export HIVE_HOME=/home/alvin/soft/hive/apache-hive-2.3.7-bin
export PATH=$HIVE_HOME/bin:$PATH
```

```
source ~/.bashrc
start-dfs.sh
start-yarn.sh
```

```
cp conf/spark-env.sh.template conf/spark-env.sh
vi conf/spark-env.sh
```

```
export SPARK_DIST_CLASSPATH=$(hadoop classpath)
#export SPARK_DIST_CLASSPATH=$(hadoop classpath):$HIVE_HOME/lib/*
```

```
cp $HADOOP_HOME/etc/hadoop/core-site.xml $SPARK_HOME/conf/
cp $HADOOP_HOME/etc/hadoop/hdfs-site.xml $SPARK_HOME/conf/
cp $HIVE_HOME/conf/hive-site.xml $SPARK_HOME/conf/
```


```
hdfs dfs -mkdir -p /user/alvin/
hdfs dfs -put README.md /user/alvin/
./bin/spark-shell
```

```scala
val textFile = spark.read.textFile("README.md")
textFile.count()
textFile.first()
val linesWithSpark = textFile.filter(line => line.contains("Spark"))
textFile.filter(line => line.contains("Spark")).count()
```

## [Spark SQL, Datasets, and DataFrames](https://spark.apache.org/docs/2.4.6/sql-programming-guide.html)

### [Data Sources](https://spark.apache.org/docs/2.4.6/sql-data-sources.html)

#### [Hive Tables](https://spark.apache.org/docs/2.4.6/sql-data-sources-hive-tables.html)

Configuration of Hive is done by placing your `hive-site.xml`, `core-site.xml` (for security configuration), and `hdfs-site.xml` (for HDFS configuration) file in `conf/`.

```
 <property>
    <name>hive.metastore.uris</name>
    <value>thrift://localhost:9083</value>
 </property>
```

