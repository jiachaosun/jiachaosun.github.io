---
layout: post
title: 简单谈谈相似度这个概念
categories: maths dataming
---

在推荐系统和聚类算法中，非常核心的一个因素是选择正确的相似度（距离测度）。
选择不同的相似度，不但可能会带来完全不同的结果，也可以影响着算法的效率和性能等。

### 推荐系统中的相似度

推荐系统这个应用，几乎每个人都接触或者使用过，无论是著名的amazon.com的“买了这个东东的顾客也买了。。。”，
还是豆瓣电影中“喜欢这部电影的人也喜欢 · · · · · ·”

![123](/asserts/imgs/dataming-similarity/amazon.png)
![123](/asserts/imgs/dataming-similarity/douban.png)

都是把你选择（当前浏览）的页面物品相似的人（或物）的其他选择推荐给你。
那如何选择正确的人和物呢，自然就是根据之前的历史行为或者物品的其他属性来计算相似度高的结果啦。

### 聚类中的相似性度量

聚类算法中同样也非常需要正确的相似性度量。
在一堆x-y坐标点的数据中，用简单的欧式距离就可以计算出它们之间的距离。
而在复杂的文本分类中就需要许多前期预处理工作，也要使用稍微复杂一点的距离测度----余弦。

![123](/asserts/imgs/dataming-similarity/QQ截图20141207181406.png)

三、常用相似度距离公式

1.欧式距离公式（Euclidian distance）
这是最简单一个距离公式：

![123](/asserts/imgs/dataming-similarity/QQ截图20141207174841.png)

2.平方欧式距离

这个距离公式是欧式距离的平方：

![123](/asserts/imgs/dataming-similarity/QQ截图20141207175000.png)

3.曼哈顿距离（Manhattan distance）

这是个有趣的公式，它采用两点之间的坐标查的绝对值。

![123](/asserts/imgs/dataming-similarity/QQ截图20141207175128.png)

区别如下：

![123](/asserts/imgs/dataming-similarity/QQ截图20141207175122.png)

4、余弦距离（cosine similiarity）

我们可以把它们想象成空间中的两条线段，都是从原点（[0, 0, ...]）出发，指向不同的方向。

两条线段之间形成一个夹角，如果夹角为0度，意味着方向相同、线段重合；如果夹角为90度，意味着形成直角，方向完全不相似；如果夹角为180度，意味着方向正好相反。因此，我们可以通过夹角的大小，来判断向量的相似程度。夹角越小，就代表越相似。

![123](/asserts/imgs/dataming-similarity/bg2013032002.png)

公式：

![123](/asserts/imgs/dataming-similarity/bg2013032007.png)

5、Jaccard距离

余弦距离无法判断向量的长度，而欧式距离无法判断向量的夹角。
正好jaccard距离可以同时考虑夹角和距离。

![123](/asserts/imgs/dataming-similarity/QQ截图20141207180016.png)

###总结

上面介绍了我们日常使用的主要距离公式，不同的距离公式会在同一数据集表现出非常另外意外的输出。

我们必须不断测试调整，才能在质量和效率上得到最满意的结果。
