---
layout: page
title: StandardLib
category: note
tags: Python
---

* content
{:toc}


# json

Import

```python
import json

data = {
    'name' : 'ACME',
    'shares' : 100,
    'price' : 542.23
	}
```

Working with string

```python
json_str = json.dumps(data)
data = json.loads(json_str)
```


Working with file

```python
with open('data.json', 'w') as f:
    json.dump(data, f)
with open('data.json', 'r') as f:
    data = json.load(f)
```



# Configparser

```python
import configparser

config = configparser.ConfigParser()
config.read('config.ini')
PAR1 = config.get('Constants', 'PAR1')
```

