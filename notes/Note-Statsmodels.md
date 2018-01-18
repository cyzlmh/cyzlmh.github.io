---
layout: page
title: Statsmodels
category: note
tags: Python
---

* content
{:toc}


# Linear regression

import

```python
import statsmodels.api as sm
```

OLS

```
XX = np.random.rand(10)
XX = sm.add_constant(XX)
YY = np.random.rand(10)
results = sm.OLS(YY, XX).fit()
intercept, slope = results.params
r_squared = results.rsquared
```

