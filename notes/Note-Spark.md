---
layout: page
title: Spark
category: note
tags: Hadoop
---

* content
{:toc}

# Spark

## PySpark

### initialize and stop

```python

# 使用spark context
from pyspark import SparkContext
sc = SparkContext()
# some code
sc.stop()


# 使用spark sql
from pyspark.sql import SparkSession
spark = SparkSession \
    .builder \
    .appName("test") \
    .config("spark.some.config.option", "some-value") \
    .getOrCreate()
# some code
spark.stop()

```

### read and write

```python

# load parquet file
data = spark.read.parquet("file_name.parquet")

# show dataframe
data.show()

# write json file
data.write.json("filename.json")

```