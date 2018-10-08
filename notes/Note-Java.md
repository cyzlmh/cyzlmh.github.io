---
layout: page
title: Java
category: note
tags: Java
---

* content
{:toc}

# Java

## Maven

### Install Maven

1. 下载Binary tar.gz archive文件
2. 安装

``` shell

mkdir ~/.java
cd ~/.java
tar zxvf apache-maven-3.5.4-bin.tar.gz

vi ~/.bash_profile

export MAVEN_HOME=/Users/yizhenchen/.java/apache-maven-3.5.4
export M2_HOME=$MAVEN_HOME
export PATH=$PATH:$MAVEN_HOME/bin

source ~/.bash_profile
```

3. 检查是否安装成功

``` shell

mvn -v
echo $M2_HOME

```

首次运行完mvn后会在"~"目录下创建".m2"文件夹，作为本地仓库

### 配置Maven

修改配置文件

- $MAVEN_HOME/conf/settings.xml
- ~/.m2/settings.xml

### 使用Maven

在项目中添加pom.xml文件

- groupId: 一般为package名
- artifactId： 一般为项目名
- version： 版本

目录结构：

- Project
  - pom.xml
  - src
    - main
      - java
      - resources
    - test
      - java
      - resources
  - target

```xml
<groupId>here.there</groupId>
<artifactId>MavenTest</artifactId>
<version>1.0</version>
```

命令

- clean：清空target目录
- complie：编译工程
- package：执行打包
- install：发布到仓库

#### hive udf dependency

```xml
<dependencies>
  <dependency>
    <groupId>org.apache.hive</groupId>
    <artifactId>hive-exec</artifactId>
    <version>0.12.0</version>
  </dependency>
  <dependency>
    <groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-common</artifactId>
    <version>2.5.0</version>
  </dependency>
</dependencies>
```