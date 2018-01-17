---
layout: post
title: Note-Pandas
category: note
tags: Python
---

* content
{:toc}


# Import

```python
import pandas as pd
from pandas import Series, DataFrame
```

# Read from file

```python
df = pd.read_csv(filename, sep=',')
df = pd.read_table(filename, sep=',',
                   parse_dates=["Date & Time Stamp"],
                   index_col="Date & Time Stamp")
df = pd.read_fwf(filename,
                 widths=[width1, width2, width3]
                 names=['col1', 'col2', 'col3'])
```

# Save to file

```python
df.to_csv('path/filename')
df.to_pickle('path/filename')
```

# Set index

```python
df.set_index(column, inplace=True)
```

# Quick check

```python
df.head(num_of_row)
df.tail(num_of_row)
df.describe()
```

# Indexing

```python
df.label
df['label']
df[i]
# select with label
df.loc['label']
# select with index
df.iloc[i]
# select with label/index
df.ix['label']
df.ix[i]
```

# Select column

```python
df.loc[:,'lable']
```

# Rename columns

```python
df.rename(columns={"old_name":"new_name"})
```

# Shift index

```python
# shift i row
df.shift(i)
# moving average
(df + df.shift(-1) + df.shift(-2)) / 3
```

# Sort

```python
df.sort_index()
df.sort_values(by='Country')
```

# Binning

```python
pd.cut(L, bin_num)
```

# Groupby

```
df.groupby(col).mean()
df.groupby([col1, col2]).mean()
df.groupby(col).aggregate(['count', 'mean'])

# find the min row of the group
df.loc[df.groupby(['Serial_Num']).apply(lambda x: x['Distance'].idxmin())]
```

# Resample

```python
df.resample('1D').max()		#max of one day
df.resample('2M').mean()	#mean of two months
```



