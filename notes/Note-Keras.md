---
layout: page
title: Keras
category: note
tags: Python
---

## sequential model

``` python
from keras.models import Sequential
model = Sequential()

```

## general model

``` python
from keras.models import Model

```

## check GPU

``` shell
# check if tensorflow is gpu version
pip list | grep tensorflow
```

``` python
K.tensorflow_backend._get_available_gpus()

sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))

from tensorflow.python.client import device_lib
print(device_lib.list_local_devices())

with tf.device('/gpu:0'):
    a = tf.constant([1., 2., 3.], shape=[1,3], name='a')
    b = tf.constant([1., 2., 3.], shape=[1,3], name='b')
    c = tf.matmul(a, b)

with tf.Session() as sess:
    print(sess.run(c))
```