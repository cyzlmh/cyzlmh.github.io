---
layout: page
title: Hadoop
category: note
tags: Hadoop
---

* content
{:toc}

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
