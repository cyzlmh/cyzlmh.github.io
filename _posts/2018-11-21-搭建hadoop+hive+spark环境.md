---
layout: post
title:  "搭建hadoop+hive+spark环境"
date:   2018-11-21 00:00:00 +0800
categories: post
tag: 笔记
---

# 安装mysql

``` shell
sudo apt-get update
sudo apt-get install mysql-server
sudo apt-get install mysql-client

# 启动
sudo mysql -u root -p


CREATE DATABASE hive;
USE hive;
CREATE USER 'hive'@'localhost' IDENTIFIED BY 'hive';
GRANT ALL ON hive.* TO 'hive'@'localhost' IDENTIFIED BY 'hive';
GRANT ALL ON hive.* TO 'hive'@'%' IDENTIFIED BY 'hive';
FLUSH PRIVILEGES;

```

# 安装hadoop

``` shell
sudo apt-get install ssh
sudo apt-get install rsync

tar -xzvf hadoop-2.7.6.tar.gz
ln -s /home/yzchen/hadoop hadoop

export JAVA_HOME=/home/yzchen/jdk
export HADOOP_HOME=/home/yzchen/hadoop
export PATH="/home/yzchen/hadoop/bin:$PATH"

mkdir home/yzchen/hadoop/tmp
mkdir home/yzchen/hadoop/hdfs
mkdir home/yzchen/hadoop/hdfs/name
mkdir home/yzchen/hadoop/hdfs/data
```

## 配置ssh本地登录

``` shell
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys

ssh localhost
```

## 配置文件

etc/hadoop/core-site.xml:

``` xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/home/yzchen/hadoop/tmp</value>
    </property>
</configuration>
```

etc/hadoop/hdfs-site.xml:

``` xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>/home/yzchen/hadoop/hdfs/name</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>/home/yzchen/hadoop/hdfs/data</value>
    </property>
</configuration>
```

etc/hadoop/yarn-site.xml:

``` xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```

## 启动

``` shell
bin/hdfs namenode -format

sbin/start-dfs.sh
sbin/start-yarn.sh
```

## 自动启动

添加文件hadoop、yarn到/etc/init.d

``` shell
#! /bin/sh
### BEGIN INIT INFO
# Provides:             hadoop
# Required-Start:       $local_fs $remote_fs $network $syslog
# Required-Stop:        $local_fs $remote_fs $network $syslog
# Default-Start:        2 3 4 5
# Default-Stop:         0 1 6
# Short-Description:    start hadoop
### END INIT INFO
export JAVA_HOME=/home/yzchen/jdk
case $1 in
    start) su yzchen /home/yzchen/hadoop/sbin/start-dfs.sh;;
    stop) su yzchen /home/yzchen/hadoop/sbin/stop-dfs.sh;;
    *) echo "require start|stop" ;;
esac
exit 0
```

``` shell
#! /bin/sh
### BEGIN INIT INFO
# Provides:             yarn
# Required-Start:       $local_fs $remote_fs $network $syslog
# Required-Stop:        $local_fs $remote_fs $network $syslog
# Default-Start:        2 3 4 5
# Default-Stop:         0 1 6
# Short-Description:    start yarn
### END INIT INFO
export JAVA_HOME=/home/yzchen/jdk
case $1 in
    start) su yzchen /home/yzchen/hadoop/sbin/start-yarn.sh;;
    stop) su yzchen /home/yzchen/hadoop/sbin/stop-yarn.sh;;
    *) echo "require start|stop" ;;
esac
exit 0
```

添加自动启动服务

``` shell
export LC_ALL=en_US.UTF-8
sudo chmod 755 hadoop
sudo update-rc.d hadoop defaults 90
sudo chmod 755 yarn
sudo update-rc.d yarn defaults 90
```

# 安装hive

``` shell
wget http://mirrors.shu.edu.cn/apache/hive/hive-2.3.4/apache-hive-2.3.4-bin.tar.gz
tar -xzvf apache-hive-2.3.4-bin.tar.gz

ln -s /home/yzchen/apache-hive-2.3.4-bin hive

export HIVE_HOME="/home/yzchen/hive"
export path="/home/yzchen/hive/bin:$PATH"
```

## 配置文件

``` shell
cd hive/conf
cp hive-default.xml.template hive-site.xml
# 讲相对路径${system:java.io.tmpdir{/${system:user.name}改为绝对路径/home/yzchen/hive/tmp/hive
```

``` xml
<property>
    <name>hive.exec.scratchdir</name>
    <value>/user/hive/tmp</value>
</property>
<property>
    <name>hive.metastore.warehouse.dir</name>
    <value>/user/hive/warehouse</value>
</property>
<property>
    <name>hive.querylog.location</name>
    <value>/user/hive/log</value>
</property>
<property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:mysql://localhost:3306/hive?createDatabaseIfNotExist=true&amp;characterEncoding=UTF-8&amp;useSSL=false</value>
</property>
<property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.jdbc.Driver</value>
</property>
<property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>hive</value>
</property>
<property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>hive</value>
</property>
<property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>hive</value>
</property>
<property>
    <name>hive.metastore.uris</name>                                                           <value>thrift://localhost:9083</value>
</property>
```

下载文件mysql-connector-java-5.1.43.jar放到hive/lib下

## 初始化元数据库

``` shell
sudo apt-get install libmysql-java
ln -s /usr/share/java/mysql-connector-java.jar $HIVE_HOME/lib/mysql-connector-java.jar
mysql -u root -p
source hive/scripts/metastore/upgrade/mysql/hive-schema-2.3.0.mysql.sql

bin/schematool -dbType mysql -initSchema
```

## 启动metastore服务

``` shell
hive --service metastore
```

# 安装scala

``` shell
tar -xvzf scala-2.11.8.tgz
ln -s /home/yzchen/scala-2.11.8 scala
export SCALA_HOME="/home/yzchen/scala"
export PATH="$SCALA_HOME/bin:$PATH"
```

# 安装spark

``` shell
wget https://archive.apache.org/dist/spark/spark-2.0.2/spark-2.0.2-bin-hadoop2.7.tgz
tar -xvzf spark-2.0.2-bin-hadoop2.7.tgz
ln -s /home/yzchen/spark-2.0.2-bin-hadoop2.7 spark
export SPARK_HOME=/home/yzchen/spark
export PATH=$SPARK_HOME/bin:$PATH
```

修改配置文件

``` shell
cp spark-env.sh.template spark-env.sh
vi spark-env.sh

export SPARK_MASTER_IP=cloud #主机名
export SPARK_WORKER_CORES=2
export SPARK_WORKER_MEMORY=2g
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
```

修改slaves文件

``` shell
cp slaves.template slaves
vi slaves

127.0.1.1 cloud
```

## python3.6不兼容

用conda降级为3.5

``` shell
conda create -n python35
source activate python35
3. 在这个新的开发环境中安装python 3.5: 
```

## 设置ipython

在文件中spark/conf/spark-env.sh加入

``` shell
export PYSPARK_DRIVER_PYTHON="ipython"
```

## 在jupyter中调用pyspark

``` python
import sys, os
spark_home = os.environ.get("SPARK_HOME", None)
if not spark_home:
    raise ValueError("spark environment variable not set")
sys.path.insert(0, os.path.join(spark_home, "python"))
sys.path.insert(0, os.path.join(spark_home, "python/lib/py4j-0.10.4-src.zip"))
exec(open(os.path.join(spark_home, "python/pyspark/shell.py")).read())
```

## 连接hive

``` shell
# 复制hive/conf/hive-site.xml到spark/conf下
cp hive/conf/hive-site.xml sprak/conf
# 复制mysql-connector-java.jar到spark/jars下
ln -s /usr/share/java/mysql-connector-java.jar $SPARK_HOME/jars/mysql-connector-java.jar
# 启动spark
spark-shell --jars $SPARK_HOME/jars/mysql-connector-java.jar
```