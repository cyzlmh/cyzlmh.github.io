---
layout: page
title: Hive
category: note
tags: Hadoop
---

* content
{:toc}

## CLI

``` shell
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
    name    STRING COMMENT 'the name',
    salary    FLOAT COMENT 'the salary'
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
    app_name    STRING
    session_id    LONG
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
    exchange    STRING
    price        FLOAT
    )
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
LOCATION '/data/stocks';

LOAD DATA LOCAL INPATH 'data_path'
OVERWRITE INTO TABLE ext_table;
```

### 分区

```sql
# 创建分区
CREATE TABLE a_table (
    name    STRING
    )
    PARTITIONE (country = 'US', state = 'CA');

# 查看分区
SHOW PARTITIONS a_table;

# 外部表分区
CREATE EXTERNAL TABLE IF NOT EXISTS log_messages (
    message        STRING
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
ROW FORMAT DELIMITED        # 告诉hive一行数据是一个record
FIELDS TERMINATED BY '\t'    # 告诉hive列分隔符
STORED AS TEXTFILES            # 以文本格式储存
STORED AS SEQUENCEFILE        # 以二进制编码储存
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
# 先根据文件格式设置表的分隔符，再导入文件到表内
INSERT OVERWRITE LOCAL DIRECTORY '/data/home/tmp/file' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '|'
    SELECT * FROM a_table;
```

### Join

``` sql
# join = inner join
# 如果表中的key有重复，例如下面的name在某个表中有重复值，join的结果也会有重复值
select t1.name, t1.num, t2.sex from
(select name, num from tmp_yzchen_test) t1
inner join
(select name, sex from tmp_yzchen_test_2) t2
on t1.name = t2.name

# left join = left outer join
# 如果左表中的key右表没有，结果会是NULL
select t1.name, t1.num, t2.sex from
(select name, num from tmp_yzchen_test) t1
left join
(select name, sex from tmp_yzchen_test_2) t2
on t1.name = t2.name

# full join = full outer join
# 结果为两边key的并集，没有匹配上的为NULL
select t1.name, t1.num, t2.sex from
(select name, num from tmp_yzchen_test) t1
outer join
(select name, sex from tmp_yzchen_test_2) t2
on t1.name = t2.name

# join
# 笛卡尔join，没有on条件
select t1.name, t1.num, t2.sex from
(select name, num from tmp_yzchen_test) t1
join
(select name, sex from tmp_yzchen_test_2) t2
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
# assigning which python here is useless
import sys

with open("some_file", "r") as f:
    # do not add the path while opening a file
    pass

for line in sys.stdin:
    print('hello ' + line)
```

```sql
add file /absolute/path/of/udf.py
add file /other/resource/some_file -- every file used in the udf should be added

-- just write 'python' here, assigning which python here will raise error
select transform(name) using 'python udf.py' AS hello FROM test_table;
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

## 调优

### hive内存设置

```sql
set mapreduce.map.memory.mb=8072;
set mapreduce.map.java.opts=-Xmx6096m;
set mapreduce.reduce.memory.mb=8072;
set mapreduce.reduce.java.opts=-Xmx6096m;
```

### 数据倾斜

1. 使用group by代替distinct操作

```sql
set hive.map.aggr = true;
set hive.groupby.skewindata=true;
```

### Mapjoin

```sql
set hive.auto.convert.join=true; -- 自动转化为mapjoin
set hive.auto.convert.join.use.nonstaged=true; -- 在不同阶段都可以使用mapjoin
set hive.mapjoin.smalltable.filesize = 30000000; -- 设置mapjoin中表的大小最大为30M
set hive.auto.convert.join.noconditionaltask=true; -- 是否将多个mapjoin合并为一个
set hive.auto.convert.join.noconditionaltask.size=20971520; -- 多个mapjoin转换为一个时，所有小表的文件大小总和的最大值

select
/*+ MAPJOIN(t1)*/
t1.k, t2.v2
from
(select k, v1 from table1) t1
join
(select k, v2 from table2) t2
on t1.k = t2.k
```

## 其他设置

```sql
set hive.cli.print.header=true;
```

## 自定义UDF

### UDF

``` java
package yzchen;
import org.apache.hadoop.hive.ql.exec.UDF;
import java.io.UnsupportedEncodingException;

public class MyUdf extends UDF {
    public int evaluate(String myInput) throws UnsupportedEncodingException {
        int result = 0;
        return result;
    }
}
```

### UDTF

每一行输入可以返回0-N行的函数，与explode函数类似

``` java
package yzchen;
import org.apache.hadoop.hive.ql.udf.generic.GenericUDTF;
import org.apache.hadoop.hive.ql.exec.UDFArgumentException;
import org.apache.hadoop.hive.ql.exec.UDFArgumentLengthException;
import org.apache.hadoop.hive.ql.metadata.HiveException;
import org.apache.hadoop.hive.serde2.objectinspector.ObjectInspector;
import org.apache.hadoop.hive.serde2.objectinspector.ObjectInspectorFactory;
import org.apache.hadoop.hive.serde2.objectinspector.StructObjectInspector;
import org.apache.hadoop.hive.serde2.objectinspector.primitive.PrimitiveObjectInspectorFactory;
import java.util.ArrayList;
import java.util.HashSet;

public class MyUdtf extends GenericUDTF {

    // 在initialize()方法中指定输入输出参数
    @Override
    public StructObjectInspector initialize(ObjectInspector[] args) throws UDFArgumentException {
        // 检查输入参数个数
        if (args.length != 1) {
            throw new UDFArgumentLengthException("UDTF takes only one argument");
        }
        // 检查输入参数类型
        if (args[0].getCategory() != ObjectInspector.Category.PRIMITIVE) {
            throw new UDFArgumentException("UDTF takes string as a parameter");
        }

        ArrayList<String> fieldNames = new ArrayList<String>();
        ArrayList<ObjectInspector> fieldOIs = new ArrayList<ObjectInspector>();
        // 定义输出格式
        fieldNames.add("col1");
        fieldOIs.add(PrimitiveObjectInspectorFactory.javaStringObjectInspector);
        fieldNames.add("col2");
        fieldOIs.add(PrimitiveObjectInspectorFactory.javaStringObjectInspector);
        // 返回返回行的个数与类型
        return ObjectInspectorFactory.getStandardStructObjectInspector(fieldNames, fieldOIs);
    }

    // process()方法中定义处理过程，每一次forward()产生一行，如果输出是多列则返回一个数组
    @Override
    public void process(Object[] args) throws HiveException {
        String input = args[0].toString();
        String[] arr = input.split("\t");
        for (String s: arr) {
            String[] result = {"hello,", s};
            forward(result);
        }
    }

    // close()方法对需要清理的函数进行清理
    @Override
    public void close() throws HiveException {
        // do nothing
    }
}
```

使用方法

``` sql
-- 直接使用
select MyUdtf(name) as (col1, col2) from src_table;

-- 结合lateral view
select src_table.num, tmp_table.col1, tmp_table.col2
from src_table lateral view MyUdtf(name) tmp_table as col1, col2;
```
