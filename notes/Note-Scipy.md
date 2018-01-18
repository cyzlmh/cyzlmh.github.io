---
layout: page
title: Scipy
category: note
tags: Python
---

* content
{:toc}


# Integrate

Import

```python
from scipy import integrate
```

Integrate

```python
def f(x, y):
    return 6*x*y**2
integrate.nquad(f, [[0, 1],[0, 1]])
```



# Optimize

Import

```python
from scipy.optimize import brentq
```

Find root

```python
x = brentq(lambda x: x/exp(x)-k, 0, 1)
```


# Statistics

Import

```python
from scipy import stats
```

Example 1

```python
n = 5	# number of sample
xs = [0.1, 0.5, 0.9]	# percentile
rv = stats.beta(a=0.5, b=0.5)	# generate a beta distribution

print(rv.pdf(xs)) # equivalent of dbeta
print(rv.cdf(xs)) # equivalent of pbeta
print(rv.ppf(xs)) # equvialent of qbeta
print(rv.rvs(n))  # equivalent of rbeta
```

Example 2

```python
dist = stats.expon()
x = np.linspace(0,4,100)
y = np.linspace(0,1,100)

with plt.xkcd():
    plt.figure(figsize=(12,4))
    plt.subplot(121)
    plt.plot(x, expon_cdf(x))
    plt.axis([0, 4, 0, 1])
```

