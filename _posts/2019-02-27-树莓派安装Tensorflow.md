---
layout: post
title:  "树莓派安装Tensorflow"
date:   2019-02-28 00:07:57 +0800
categories: post
tags: raspberrypi
---

## 使用Conda安装

* (已失败，Miniconda会将python3.5降级为3.4，而最新版tensorflow已经不支持python3.4)*

### 安装Miniconda3

    ``` shell
    wget http://repo.continuum.io/miniconda/Miniconda3-latest-Linux-armv7l.sh
    sudo bash Miniconda3-latest-Linux-armv7l.sh

    # 添加环境变量至~/.bashrc
    export PATH="/home/pi/miniconda3/bin:$PATH"

    # 更改权限
    sudo chown pi:pi -R miniconda3

    # 换源
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
    conda config --set show_channel_urls yes

    # 更新
    conda update conda
    ```

## 使用pip安装

1. 安装pip

    ``` shell
    sudo apt-get install python-pip
    sudo apt-get install python3-pip

    # 请勿升级，升级后会报错
    #pip install --upgrade pip
    #pip3 install --upgrade pip
    ```

2. 安装tensorflow

    ``` shell
    sudo apt install libatlas-base-dev
    pip3 install tensorflow
    ```