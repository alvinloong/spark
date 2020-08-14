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

# API Docs

## [Spark Python API (Sphinx)](https://spark.apache.org/docs/2.4.6/api/python/index.html)

### [pyspark package](https://spark.apache.org/docs/2.4.6/api/python/pyspark.html)

### [pyspark.sql module](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html)

#### [Module Context](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#module-pyspark.sql)

##### [SparkSession](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SparkSession)

```
spark = SparkSession.builder \
    .master("local") \
    .appName("Word Count") \
    .config("spark.some.config.option", "some-value") \
    .getOrCreate()
```

###### [Builder](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SparkSession.Builder)

[appName](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SparkSession.Builder.appName)

```
s0 = SparkSession.builder.config("spark.app.name", "alvinapp").getOrCreate()
```

[config](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SparkSession.Builder.config)

```
from pyspark.conf import SparkConf
SparkSession.builder.config(conf=SparkConf())
<pyspark.sql.session...
```

```
SparkSession.builder.config("spark.some.config.option", "some-value")
<pyspark.sql.session...
```

[enableHiveSupport](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SparkSession.Builder.enableHiveSupport)

[getOrCreate()](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SparkSession.Builder.getOrCreate)

```
s1 = SparkSession.builder.config("k1", "v1").getOrCreate()
s1.conf.get("k1") == s1.sparkContext.getConf().get("k1") == "v1"
```

```
s2 = SparkSession.builder.config("k2", "v2").getOrCreate()
s1.conf.get("k1") == s2.conf.get("k1")
True
s1.conf.get("k2") == s2.conf.get("k2")
True
```

###### [createDataFrame](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SparkSession.createDataFrame)

```
l = [('Alice', 1)]
spark.createDataFrame(l).collect()
[Row(_1='Alice', _2=1)]
spark.createDataFrame(l, ['name', 'age']).collect()
[Row(name='Alice', age=1)]
```

```
d = [{'name': 'Alice', 'age': 1}]
spark.createDataFrame(d).collect()
[Row(age=1, name='Alice')]
```

```
/home/alvin/soft/spark/spark-2.4.6/python/pyspark/sql/session.py:346: UserWarning: inferring schema from dict is deprecated,please use pyspark.sql.Row instead
  warnings.warn("inferring schema from dict is deprecated,"
```

```
rdd = sc.parallelize(l)
spark.createDataFrame(rdd).collect()
[Row(_1='Alice', _2=1)]
df = spark.createDataFrame(rdd, ['name', 'age'])
df.collect()
[Row(name='Alice', age=1)]
```

```
from pyspark.sql import Row
Person = Row('name', 'age')
person = rdd.map(lambda r: Person(*r))
df2 = spark.createDataFrame(person)
df2.collect()
[Row(name='Alice', age=1)]
```

```
from pyspark.sql.types import *
schema = StructType([
   StructField("name", StringType(), True),
   StructField("age", IntegerType(), True)])
df3 = spark.createDataFrame(rdd, schema)
df3.collect()
[Row(name='Alice', age=1)]
```

```
spark.createDataFrame(df.toPandas()).collect()  
[Row(name='Alice', age=1)]
spark.createDataFrame(pandas.DataFrame([[1, 2]])).collect()  
[Row(0=1, 1=2)]
```

```
spark.createDataFrame(rdd, "a: string, b: int").collect()
[Row(a='Alice', b=1)]
rdd = rdd.map(lambda row: row[1])
spark.createDataFrame(rdd, "int").collect()
[Row(value=1)]
#spark.createDataFrame(rdd, "boolean").collect() 
#Traceback (most recent call last):
```

###### [range](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SparkSession.range)

```
spark.range(1, 7, 2).collect()
[Row(id=1), Row(id=3), Row(id=5)]
```

```
spark.range(3).collect()
[Row(id=0), Row(id=1), Row(id=2)]
```

###### [sql](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SparkSession.sql)

```
df.createOrReplaceTempView("table1")
#df2 = spark.sql("SELECT field1 AS f1, field2 as f2 from table1")
#df2.collect()
[Row(f1=1, f2='row1'), Row(f1=2, f2='row2'), Row(f1=3, f2='row3')]
```

###### [table](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SparkSession.table)

```
#df.createOrReplaceTempView("table1")
#df2 = spark.table("table1")
#sorted(df.collect()) == sorted(df2.collect())
True
```

##### [SQLContext](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SQLContext)

As of Spark 2.0, this is replaced by [`SparkSession`](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.SparkSession). 

##### [Row](https://spark.apache.org/docs/2.4.6/api/python/pyspark.sql.html#pyspark.sql.Row)

```
row = Row(name="Alice", age=11)
row
Row(age=11, name='Alice')
row['name'], row['age']
('Alice', 11)
row.name, row.age
('Alice', 11)
'name' in row
True
'wrong_key' in row
False
```

```
Person = Row("name", "age")
Person
<Row(name, age)>
'name' in Person
True
'wrong_key' in Person
False
Person("Alice", 11)
Row(name='Alice', age=11)
```

