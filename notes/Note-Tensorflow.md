---
layout: page
title: Tensorflow
category: note
tags: Machine Learning
---

## Tensorflow

``` python
# define constant
a = tf.constant(1)
b = tf.constant(2, name='const', shape=(3,5), dtype=tf.float64)

# define operation
add = a + b
mul = tf.square(tf.multiply(a, b))

# define placeholder
p = tf.placeholder(tf.int32)

# define variables
x = tf.get_variable('x', [], dtype=tf.int32)
y = tf.get_variable('y', shape=(3,5), initializer=tf.constant_initializer(0))

# session
a = tf.constant(1)
b = tf.constant(2)
f = a + b
sess = tf.Session()
print(sess.run(f)) # constant do not need initialization

x = tf.get_variable('x', [], dtype=tf.int32)
f = x + a
sess = tf.Session()
sess.run(x.initializer) # variable must be initialized before run
# return multiple items in a single sess.run() call, instead of making multiple calls
print(sess.run([x, f]))

p = tf.placeholder(tf.int32)
sess = tf.Session()
print(sess.run(p, feed_dict={p:2})) # placeholder must be feed in run

# assignment
# assign node is not connected to the variable, it change the variable's value by side effect
x = tf.get_variable('x', [], dtype=tf.int32)
a = tf.constant(1)
assign = tf.assign(x, a)
sess = tf.Session()
sess.run(assign)
print(sess.run(x))

# sess with block
x = tf.get_variable('x', shape=(3,5), initializer=tf.constant_initializer(0))
y = tf.get_variable('y', shape=(3,5), initializer=tf.constant_initializer(0))
f = x + y
with tf.Session() as sess:
    x.initializer.run()
    y.initializer.run()
    print(f.eval())

x = tf.get_variable('x', shape=(3,5), initializer=tf.constant_initializer(0))
y = tf.get_variable('y', shape=(3,5), initializer=tf.constant_initializer(0))
f = x * x
g = f + y
with tf.Session() as sess:
    x.initializer.run()
    y.initializer.run()
    f_val, g_val = sess.run([f, g]) # eval f and g in one run
    print(f_val, g_val)

# interative session
x = tf.get_variable('x', [], initializer=tf.constant_initializer(3))
f = x * x
sess = tf.InteractiveSession()
x.initializer.run()
print(f.eval())

# global initializer
x = tf.get_variable('x', shape=[], initializer=tf.constant_initializer(1))
y = tf.get_variable('y', shape=[], initializer=tf.constant_initializer(2))
f = x + y
init = tf.global_variables_initializer()
with tf.Session() as sess:
    init.run()
    print(f.eval())

# name scope
x = tf.get_variable('x', [], , initializer=tf.constant_initializer(1))
with tf.variable_scope('scope'):
    y = tf.get_variable('x', [], , initializer=tf.constant_initializer(2))
print(x.name, y.name)

# get graph
default_graph = tf.get_default_graph()
print(default_graph)

new_graph = tf.Graph()
with new_graph.as_default():
    x = tf.Variable(2)
print(x.graph)
```

### Save and Load a Model

``` python
# save a model
x = tf.get_variable('x', [])
y = tf.get_variable('y', [])
init = tf.global_variables_initializer()

# define the saver after every variable is defined
saver = tf.train.Saver()

sess = tf.Session()
sess.run(init)
# save the model after run
saver.save(sess, './models/test-model')


# load a model
x = tf.get_variable('x', [])
y = tf.get_variable('y', [])
saver = tf.train.Saver()
sess = tf.Session()
saver.restore(sess, './models/test-model') # no need to init
sess.run([x, y])
```

### Optimization

``` python

# autodiff
x = tf.get_variable('x', [], initializer=tf.constant_initializer(3.))
y = tf.get_variable('y', [], initializer=tf.constant_initializer(2.))
f = y*x + x*x + 3*x
grad = tf.gradients(f, [x, y])
sess = tf.Session()
sess.run(x.initializer)
sess.run(y.initializer)
print(sess.run(grad)) # return [df/dx, df/dy]

# example
k = tf.get_variable('k', [], initializer=tf.constant_initializer(0.))
b = tf.get_variable('b', [], initializer=tf.constant_initializer(0.))
init = tf.global_variables_initializer()

x = tf.placeholder(tf.float32)
y = tf.placeholder(tf.float32)
y_pred = k * x + b
loss = tf.square(y - y_pred)

optimizer = tf.train.GradientDescentOptimizer(1e-3)
train_op = optimizer.minimize(loss)

sess = tf.Session()
sess.run(init)

import random
true_k = random.random()
true_b = random.random()

for update_i in range(10000):
    input_data = random.random()
    output_data = random.random()

    _loss, _ = sess.run([loss, train_op], feed_dict={x: input_data, y: output_data})
    print(update_i, _loss)

print('True parameter: k={}, b={}'.format(true_k, true_b))
print('True parameter: k={}, b={}'.format(sess.run([m, b])))
```

### Tensorboard

``` python
from datetime import datetime
now = datetime.now().strftime("%Y%m%d%H%M%S")
root_logdir = "tf_logs"
logdir = "{}/run-{}/".format(root_logdir, now)

# end of construction phase
mse_summary = tf.summary.scalar('MSE', mse)
file_writer = tf.summary.FileWriter(logdir, tf.get_default_graph())

# execution phase
for batch_index in range(n_batches):
    X_batch, y_batch = fetch_batch(epoch, batch_index, batch_size)
    if batch_index % 10 == 0:
        summary_str = mse_summary.eval(feed_dict={X: X_batch, y: y_batch})
        step = epoch * n_batches + batch_index
        file_writer.add_summary(summary_str, step)
    sess.run(training_op, feed_dict={X: X_batch, y: y_batch})

# finally
file_writer.close()

# start the Tensorboard service
!tensorboard --logdir tf_logs/
```

## Keras

### sequential model

``` python
import keras
from keras.models import Sequential
from keras.layers import Dense

(x_train, y_train), (x_test, y_test) = keras.datasets.mnist.load_data()
y_train = keras.utils.to_categorical(y_train)

model = Sequential()
model.add(Dense(100, activation='relu', input_shape=(28*28,)))
model.add(Dense(100, activation='relu'))
model.add(Dense(10, activation='softmax'))
model.compile(loss='categorical_crossentropy', optimizer='rmsprop', metrics=['acc'])

history = model.fit(x_train.reshape(-1, 28*28), y_train, validation_split=0.1, batch_size=32, epochs=10)
```

### general model

``` python
import keras
from keras.layers import Input, Dense
from keras.models import Model

(x_train, y_train), (x_test, y_test) = keras.datasets.mnist.load_data()
y_train = keras.utils.to_categorical(y_train)

input_tensor = Input(shape=(28*28, ))
x = Dense(100, activation='relu')(input_tensor)
x = Dense(100, activation='relu')(x)
output_tensor = Dense(10, activation='softmax')(x)
model = Model(input_tensor, output_tensor)
model.compile(loss='categorical_crossentropy', optimizer='rmsprop', metrics=['acc'])

history = model.fit(x_train.reshape(-1, 28*28), y_train, validation_split=0.1, batch_size=32, epochs=10)
```

### Use Keras Layers in Tensorflow

``` python
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras.layers import Dense

x = tf.placeholder(name='x', shape=(None, 28*28), dtype=tf.float32)
hidden1 = Dense(100, activation='relu')(x)
hidden2 = Dense(100, activation='relu')(hidden1)
output = Dense(10)(hidden2)

y = tf.placeholder(tf.int64, shape=(None,))
xentropy = tf.nn.sparse_softmax_cross_entropy_with_logits(labels=y, logits=output)
loss = tf.reduce_mean(xentropy, name="loss")
# train with tensorflow
# ...
```

### Use Keras Model in Tensorflow

``` python
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

model = Sequential()
model.add(Dense(100, activation='relu', input_shape=(28*28,)))
model.add(Dense(10, activation='softmax'))

x = tf.placeholder(name='input', shape=(None, 28*28), dtype=tf.float32)
y = model(x)
```

## Check GPU

``` shell
# check if tensorflow is gpu version
pip list | grep tensorflow
```

``` python
from keras import backend as K
K.tensorflow_backend._get_available_gpus()

from tensorflow.python.client import device_lib
print(device_lib.list_local_devices())

import tensorflow as tf
sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
with tf.device('/gpu:0'):
    a = tf.constant([1., 2., 3.], shape=[1,3], name='a')
    b = tf.constant([1., 2., 3.], shape=[3,1], name='b')
    c = tf.matmul(a, b)
with tf.Session() as sess:
    print(sess.run(c))
```