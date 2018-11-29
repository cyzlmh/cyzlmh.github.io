---
layout: page
title: Python
category: note
tags: Python
---

* content
{:toc}

# Format

Float

```python
pi = 3.14159
'{0:.2f}'.format(pi)	#2位小数
'{0:05.2f}'.format(pi)	#2位小数，左边用0补齐5位
```

# pip

使用国内源

``` shell
pip install package -i https://pypi.mirrors.ustc.edu.cn/simple/
```

指定版本

``` shell
pip install pyspark==2.3.2
```

指定平台

``` shell
pip download package --platform linux_x86_64 --only-binary=:all: --python-version 27
pip download package --platform macosx-10_10_x86_64 --only-binary=:all: --python-version 27
pip download pyspark --platform win_amd64 --only-binary=:all: --python-version 27
```

# Other

Python 2与Python 3切换

```
py -2
py -3
# use pip
py -2 -m pip install
# 在文件头部添加
#! py -2
#! /usr/bin/python
#! interpreter [optional-arg]
```
