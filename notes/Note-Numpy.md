---
layout: page
title: Numpy
category: note
tags: Python
---

* content
{:toc}


# Basic

Import

```python
import numpy as np
```

Get shape

```python
x.shape
```

Meshgrid

```python
x = np.arange(-5, 5, 0.1)
y = np.arange(-5, 5, 0.1)
xx, yy = np.meshgrid(x, y)
z = np.sin(xx**2 + yy**2) / (xx**2 + yy**2)
h = plt.contourf(x,y,z)
```



# Random
Generate random samples

```python
# generate random integers over [low, high) with size d0*d1 from a uniform dist
x = np.random.randint(low, high, (d0, d1))
# generate d0*d1*d2 samples from a uniform distribution over [0, 1)
x = np.random.rand(d0, d1, d2)
# generate d0*d1*d2 samples from a normal distribution over [0, 1)
x = np.random.randn(d0, d1, d2)
# generate n samples from a uniform distribution over [0, 1)
x = np.random.random(n)
x = np.random.sample(n)
x = np.random.random_sample(n)
x = np.random.randf(n)
```


Make random choice

```python
np.random.choice(4, 12, p=[.4, .1, .1, .4])
reps = 10000
xb = np.random.choice(x, (n, reps))
```

Normal distribution

```python
μ = 100
σ = 15
n = 10000
x = np.random.normal(μ, σ, n)
```

Sample from a given distribution

```python
gb = np.random.gumbel(size=1000)
plt.hist(gb, bins=20, histtype='step', normed=True, linewidth=1)
```

