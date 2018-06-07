---
layout: post
title:  "这些年看过的电影"
date:   2018-01-31 00:00:00 +0800
categories: post
tag: Movies
---

从我09年大一时开始用豆瓣电影标记第一部电影，《飞屋环游记》记起，到现在快十年了。得益于这十年来，基本上每看完一部电影都会去豆瓣打个分、看看影评的习惯，现在豆瓣里一共标记了XX部电影。当然这个数字不精确，有时候看过的电影会忘记标记，有时则会顺手标一下小时候看过的电影。我也不算是“电影爱好者”，因为我看电影大多是为了放松娱乐，对电影技法什么的也一窍不通，却也看了这么多电影了。这些电影中，有的场景印象深刻，有的已经不记得讲了什么。虽然不可能一部一部回顾，但我也想知道这些年我都看了哪些电影，我喜欢过什么样的电影。

于是我用爬虫把标记过电影的信息抓了下来。豆瓣对爬虫比较友好，用的requests+lxml就简单地爬下来了，然后将数据存到了MongoDB里。

爬取的信息包括名字，上映时间，我标记的时间，国家，语言，导演，演员，编剧，评分，时长等，如下：

![data_demo](/assets/watched-movies/data_demo.png)

共爬取页面606条，其中电影551部，电视剧55部。累计看片时长达1489小时。如果说一天除去吃饭睡觉能看12个小时，那么相当于连续看了124天。

![pie_type](/assets/watched-movies/pie_type.png)

由于小时候看的影视剧记忆有偏差且数据不全，下面只分析09年以后看过的电影。

这些电影的自评与平均评分分布，看的大部分还比较喜欢。（我是这么分的：5星=9~10分，4星=8~9分，3星=7~8分，2星=6~7分，1星是小于6分的）。

![rating_distribution](/assets/watched-movies/rating_distribution.png)

我的口味与豆瓣群众们还是有些不同的，自评与平均评分相关性看起来不高。

![rating_distribution_2](/assets/watched-movies/rating_distribution_2.png)

按国家统计看过的电影数，美国片看的最多，毕竟好莱坞在电影行业是影响力太大。另外日本和法国片也看的不少，除了学语言的因素，也反映了我对日法文化的亲近感。其他小众国家中，韩国片和台湾片的评价还不错。不过也有可能是看的少，只看了最好看的一些。

![country_count](/assets/watched-movies/country_count.png)

按类型统计看过的电影，最爱剧情片，其次喜剧片，我的评分也都高于平均值。此外是冒险、动作、爱情、动画片，从平均评分看，爱爱情片动画片不爱动作片科幻片。符合我小清新的气质~

![genre_count](/assets/watched-movies/gerne_count.png)

用wordcloud做了标签的词云，基本上和类型的分类差不多。话说“同性“、”情色”的小字是似乎暴露了什么。

![tag_wordcloud](/assets/watched-movies/tag_wordcloud.png)

看过各导演作品数量统计，最多的是宫崎骏，他的每一部基本上都看过了。8部平均4.2分暴露了我是诺兰粉。最喜欢的应该是矢口史靖，平均分最高。另一个均分上4的是土井裕泰，他是《花水木》、加贺系列剧场版、垫底辣妹，还有一些电视剧的导演。平均分最低的两个山本泰一郎和静野孔文似乎是柯南剧场版的导演，忽略吧。剩下的看得多但是均分不高的多是一些系列片的导演，例如《X战警》、《冰河世纪》、《神偷奶爸》等。

![favorite_director](/assets/watched-movies/favorite_director.png)

看过最多的演员的电影。居然看了这么多斯嘉丽的电影，不过斯坦·李也很多，应该是漫威看多了，当然她还有很多别的不错的电影。高山南、山口胜平、山崎和佳奈、林原惠美都是柯南的配音。迈克尔·凯恩是蝙蝠侠的管家，常年出现在诺兰的片子里，伊恩·麦克莱恩是甘道夫，竹中直人是矢口史靖和很多日本电影常见的配角。Marion Cotillard是法国女神。中国的演员上榜的只有成龙和张涵予。这里故意把小时候看的筛掉了，不然周星驰肯定是第一。

![favorite_actor](/assets/watched-movies/favorite_actor.png)

看过的最流行的电影排行：

![most_popular](/assets/watched-movies/most_popular.png)

看过的最小众的电影排行，都是些法国小众片：

![less_popular](/assets/watched-movies/less_popular.png)

看过的老片子：

![old_movie](/assets/watched-movies/old_movie.png)

看过的片子都是哪些年的？看了52部2013年的片子，我也不知道为啥那一年的比较多哈。

![year_distribution](/assets/watched-movies/year_distribution.png)

按照我标记的时间，作出的观影时间按年与按月的分布。由于标记时间和观看时间很可能不一致，所以看看就好。2013年看的最多，可能因为在法国的最后一个学期比较懒散，平均两三天就看一部。从月份上看1月、6月、10月的比较多，可能与假期档上映电影多或放假时间多有关。

![watch_date_distribution](/assets/watched-movies/watch_date_distribution.png)

# 一些有意思的事情

其中有一个电影没爬到评分，不过点进去一看就知道怎么回事了。它叫《坚果大爷》。

还有一部电影在“看过”的列表里面存在，但是却找不到页面了，他就是《The interview》。至于为什么大家都懂。

------

最后，附上我所有的5星电影名单