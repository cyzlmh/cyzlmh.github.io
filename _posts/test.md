---
layout: test
title:  "Welcome to Jekyll!"
date:   2018-01-14 13:16:58 +0800
categories: jekyll update
published: false

test:   "some little test"
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight python %}
def print_hi(name)
    print("Hi, {}}".format(name))

print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.

import numpy as np
a = np.linspace(0, 1, 100)

{% endhighlight %}

```python
def print_hi(name)
    print("Hi, {}}".format(name))

print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.

import numpy as np
a = np.linspace(0, 1, 100)
```

```sql
SELECT * FROM Table
```


Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

{{ page.test }}

![pic]({{ "/assets/0.png" | absolute_url }})
