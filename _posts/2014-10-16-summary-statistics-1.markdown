---
layout: post
title:  "样本统计数据"
date:   2014-10-15 16:25:00
categories: statistics
---

样本统计数据可以简单理解为用几个数来概括统计大量数据的一些性质。

以下内容基于如下数据(全部基于R语言)。

![](/asserts/imgs/s-s-1.png)

{% highlight R %}
v <- c(1,2,3,3,6,2,5,7,2)
#均值（mean）
> mean(v)
[1] 3.444444

#中位数（median,样本从小到大排列后中间的那个值）
> median(v)
[1] 3

#PS:当数据的离散程度比较高时，一般都使用中位数。
#说明数据的离散程度，需要使用方差和标准差。

#方差
{% endhighlight %}


