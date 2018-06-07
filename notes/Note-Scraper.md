---
layout: page
title: Scraper
category: note
tags: Python
---

* content
{:toc}



# urllib

*Python built-in lib for web requesting*

Import

```python
from urllib.request import urlopen
from urllib.request import urlretrieve
from urllib.error import HTTPError
```
Open url

```python
page = urlopen(URL)
```

# Requests

*HTTP for human*

Import

```python
import requests
```

get/post

```python
r = requests.get(URL)
r = requests.post(URL)
```
Add Headers

```python
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36',
    'Accept': 'text/html, application/xhtml+xml, application/xml; q=0.9, img/webp,*/*; q=0.8',
    'Host': 'www.zhihu.com',
    'Referer': 'https://www.zhihu.com/'}
r = requests.get(URL, headers=headers)
```

Add cookies

```python
cookies = dict(cookies_are='working')
r = requests.get(URL, cookies=cookies)
# using cookie jar
jar = requests.cookies.RequestsCookieJar()
jar.set('tasty_cookie', 'yum', domain='httpbin.org', path='/cookies')
jar.set('gross_cookie', 'blech', domain='httpbin.org', path='/elsewhere')
r = requests.get(url, cookies=jar)
```

Check results

```python
r.status_code
r.headers
r.text
```

Build a session

```python
s = requests.session(URL)
```

Post with string data

```python
data = "some string"
r = requests.post(URL, data=data)
```

Post with json data

```python
import json
data = json.dumps({key1:item1, key2:item2})
r = requests.post(URL, data=data)
```

Post with multi-part files

```python
files = {"files":("test.pdf", open("test.pdf", "rb"),"application/pdf"), 
        "info":(None, "information")}
```

Download file

```python
report = session.get(url, stream=True)
with open("report.pdf", "wb") as f:
    for chunk in report.iter_content(chunk_size=128):
        f.write(chunk)
```

Use proxy

```python
proxies = {'http':'http://proxy:port',
    'https':'https://proxy:port'
    }
r = requests.get(URL, proxies = proxies)
```

# BeautifulSoup 4

*page parser*

Import

```python
from bs4 import BeautifulSoup
```

Parser page

```python
bsObj = BeautifulSoup(page.read(), 'html.parser')
```

Find element

```python
targets = bsObj.findAll("span", {"class":"a_cover"})
```

# lxml

*page parser with xpath*

Import

```python
from lxml import etree
```

Parser

```python
page = etree.HTML(response.text)
```

Find elements using xpath

```python
# select all subelements beneath the current node ("//") with the given tag ("span") which the given attribute ("[@class]") has the given value ("className")
elems = page.xpath('//span[@class="className"]')
# select all elements that has a child element named tag
elems = page.xpath('[tag]')
for elem in elems:
    # get the tag, text, and attribute of an element
    print(item.tag, item.text, item.get('attrib'))
# get the text directly
elems = page.xpath('//span[@class="className"]/text()')
# position
elems = page.xpath('//span[location()=1]')
```

# Selenium

*fake browser*

Import

```python
from selenium import webdriver
```

Initialize

```python
driver = webdriver.PhantomJS(executable_path=path)
```

Get page

```python
driver.get(URL)
```

Set headers

```python
webdriver.DesiredCapabilities.PHANTOMJS['phantomjs.page.settings.userAgent'] = 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.95 Safari/537.36'
webdriver.DesiredCapabilities.PHANTOMJS['phantomjs.page.customHeaders.Accept-Language'] = 'en-US,en;q=0.6'
```

# gevent

*协程爬取*

import

```python
import gevent
import gevent.queue
import gevent.pool
import gevent.monkey
```

Overwrite incompatible functions built-in Python

```python
gevent.monkey.patch_all()
```

Built a pool with assigned threads

```python
gevent_pool = gevent.pool.Pool(20)
```

Map function to each thread

```python
threads = []
threads.append(gevent.spawn(function, *args))
gevent.joinall(threads)
```

# Other

Wait random time

```python
import random
import time
time.sleep(random.random())
```

Encode string into urlcode

```python
from urllib.parse import quote
s = quote(s)
from urllib.parse import quote
```