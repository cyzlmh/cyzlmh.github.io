---
layout: page
title: Hadoop
category: note
tags: Others
---

* content
{:toc}

# Hadoop

```shell
# 查看hdfs文件系统
hadoop fs -ls <dir>

# 查看文本文件
hadoop fs -cat <filename>
hadoop fs -cat <filename> | head

# 查看压缩文件
hadoop fs -text <filename>

# 查看文件大小
hadoop fs -du <dir>
```

其他命令可以看：[hadoop shell commands](https://hadoop.apache.org/docs/r1.0.4/cn/hdfs_shell.html)

# Hive

## CLI

```shell
# 启动cli
hive
# or
hive --service cli

# 定义变量
hive --define A=B
# or
hive -d A=B

# 执行SQL语句
hive -e SELECT * FROM a_table

# 运行文件
hive -f filename
# or
hive
	source filename;

# 运行初始化文件
# 可以将初始化内容加到$HOME/.hiverc文件中
hive -i filename

# 历史记录保存在$HOME/.hivehistory文件中

# 配置文件在$HIVE_HOME/conf/hive-default.xml.template

# 配置
hive --hiveconf option_A=true
hive
	set hiveconf:option_A=trues;
	
# 执行jar应用
hive --service jar

# 执行shell命令，前面加!
hive
	!pwd;

# 执行hadoop命令，去掉前面的hadoop
hive
	fs -ls /;
	
# 注释
-- 注释内容前面加'--'
```



## Database操作

```sql
# 查看database列表
SHOW DATABASES;
# 创建database
CREATE DATABASE a_database;
# 查看信息
DESCRIBE DATABASE a_database;
# 使用database
USE a_database;
# 删除database
DROP a_database；
# 修改属性
ALTER DATABASE a_database SET DBPROPERTIES('edited-by' = 'ychen');
```

## Table操作

```sql
# 查看table列表
SHOW TABLES;
SHOW TABLES IN a_database;
# 查看表信息
DESCRIBE a_table;
DESCRIBE a_database.a_table;
DESCRIBE a_table.a_column; # 查看某一列
# 创建表
CREATE TABLE IF NOT EXISTS a_table (
	name	STRING COMMENT 'the name',
	salary	FLOAT COMENT 'the salary'
    )
    COMMENT 'Description of the table'
    TBLPROPERTIES ('creator'='me')
    LOCATION '/user/hive/warehouse/a_database/a_table';
# 删除表
DROP TABLE IF EXISTS a_table;
# 重命名表
ALTER TABLE a_table RENAME TO b_table;
# 修改列
ALTER TABLE a_table
    CHANGE COLUMN old_column new_column INT # 重命名列
    AFTER other_column; # 修改位置
# 增加列
ALTER TABLE a_table ADD COLUMNS (
	app_name	STRING
	session_id	LONG
	)
# 删除与替换列
ALTER TABLE a_table REPLACE COLUMNS (
    a_column INT，
    b_column STRING
	)
```

### 外部表

Hive没有所有权的数据，但是可以进行查询

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS ext_table (
	exchange	STRING
	price		FLOAT
	)
	ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
	LOCATION '/data/stocks';
```

### 分区

```sql
# 创建分区
CREATE TABLE a_table (
	name	STRING
	)
	PARTITIONE (country = 'US', state = 'CA');

# 查看分区
SHOW PARTITIONS a_table;

# 外部表分区
CREATE EXTERNAL TABLE IF NOT EXISTS log_messages (
	message		STRING
	)
	PARTITIONED BY (year INT, month INT, day INT);

# 增加分区
ALTER TABLE log_messages ADD PARTITION(year=2012, month=1, day=2)
	LOCATION 'hdfs://master_server/data/log_messages/2012/01/02';
# 删除分区
ALTER TABLE log_messages DROP IF EXISTS PARTITION(year=2011, month=12, day=2);
```

### 文件格式

```sql
ROW FORMAT DELIMITED		# 告诉hive一行数据是一个record
FIELDS TERMINATED BY '\t'	# 告诉hive列分隔符
STORED AS TEXTFILES			# 以文本格式储存
STORED AS SEQUENCEFILE		# 以二进制编码储存
```

### 导入导出数据


```sql
# 导入本地文件
LOAD DATA LOCAL INPATH '${env:HOME}/data'
	OVERWRITE INTO TABLE a_table

# 导入HDFS文件，会移动，而不是复制数据
LOAD DATA INPATH 'hdfs:path...'
	OVERWRITE INTO TABLE a_table

# 通过查询导入文件
INSERT OVERWRITE TABLE a_table
	SELECT cmn1, cmn2 FROM other_table;
	
# 通过查询创建表
CREATE TABLE a_table 
	AS SELECT cmn1, cmn2 FROM other_table;

# 导出到本地文件夹
INSERT OVERWRITE LOCAL DIRECTORY '/data/home/tmp/file'
  SELECT cmn1, cmn2 FROM a_table;
# 导出到HDFS文件夹
INSERT OVERWRITE DIRECTORY '/tmp/ds'
  SELECT cmn1, cmn2 FROM a_table；

# 设置文件分隔符
INSERT OVERWRITE LOCAL DIRECTORY '/data/home/tmp/file' 
	ROW FORMAT DELIMITED FIELDS TERMINATED BY '|'
	SELECT * FROM a_table;

```

### Functions


```sql
# 函数文档
DESCRIBE FUNCTION concat;

# add user-define-function
add jar /usr/local/app/secure_jar/phc_udf.jar;
create temporary function keybuilder as 'geodb.keybuilder';
```

python udf
```python
#!python
import sys

for line in sys.stdin:
	print('hello ' + line)
```
```sql
ADD FILE /scripts/udf.py

SELECT TRANSFORM(name) USING 'python /scripts/udf.py' AS hello FROM test_table;
```

### 抽样

```sql
SELECT * FROM a_table TABLESAMPLE(BUCKET 3 OUT OF 10 ON rand());
SELECT * FROM a_table TABLESAMPLE (0.1 PERCENT)
```


### 时间

```sql
-- current timestamp
SELECT UNIX_TIMESTAMP();
-- from string to unix time
SELECT UNIX_TIMESTAMP('2018-07-21', 'yyyy-MM-dd');
-- from unix time to string
SELECT FROM_UNIXTIME(1532102400, 'yyyy-MM-dd');

--选取日期、年月日时分秒
SELECT TO_DATE('2018-07-21 10:30:00');
SELECT YEAR('2018-07-21 10:30:00');
SELECT SECOND('2018-07-21 10:30:00');
```
