---
layout: page
title: pip & conda
category: note
tags: Others
---

* content
{:toc}

## pip

``` shell
# update
pip install --upgrade pip

# install package
pip install numpy
pip --proxy=hostname:port install requests

# check packages
pip list

# check version
pip freeze | grep [package_name]
```

## conda

``` shell
# cheak version
conda -V

#update
conda update conda

# list env
conda env list

# create new env
conda create --name myenv python=3.5

# activate env
source activate myenv

# install package
# use pip or conda, do not use them simultaneously
pip install numpy
conda install numpy

# unistall
conda unistall numpy
conda remove --name myenv numpy

# check packages
conda list

# deactivate env
source deactivate myenv

# remove env
conda env remove --name myenv
```