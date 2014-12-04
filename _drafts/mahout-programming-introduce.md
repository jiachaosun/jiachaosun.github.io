---
layout: post
title: mahout hello world + 体系介绍
categories: mahout
---

mahout 对于自己的定位是 Scalable machine learning libraries，意思是可扩展的机器学习库。

它对于数据挖掘的相关开发者来说是个非常值得使用和学习的东西，mahout不但包含了机器学习三大领域的大部分算法实习，更带给我们非常棒的分布式算法实现（on hadoop）。


这在大数据非常火爆的当下，使得 mahout 成为了算法+分布式+应用开发于一体的数据挖掘机器学习领域明日之星。

一 mahout 体系

mahout主要包括了机器学习的三大领域，分别是推荐系统+聚类+分类算法。
从代码结构也可以看出这点。

![123](/asserts/imgs/mahout/m-2-1.png)

cf.taste下主要对应的是 mahout 中的协同过滤算法实现，这些中最为人们熟悉的就是 基于用户的协同过滤 和 基于物品的协同过滤 算法。

![123](/asserts/imgs/mahout/m-2-2.png)

图中前2个类对应的是无偏好值（Preference）的场景下使用，后2个类对应的是有偏好场景下，比如对一个物品进行评分，我们可以如下表示：

![123](/asserts/imgs/mahout/m-2-3.png)


未完待续。。。
