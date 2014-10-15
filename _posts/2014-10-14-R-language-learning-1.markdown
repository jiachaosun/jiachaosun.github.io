---
layout: post
title:  "R语言学习笔记1"
date:   2014-10-14 16:27:30
categories: language R
---
最近在跟coursera.org的数据科学家工具箱课程，期间对R语言突然产生了兴趣，所以记录下一些学习R语言过程中的笔记。

##1.环境搭建

课程中的R语言开发环境需要两部分内容，一是[下载R语言运行时](http://cran.rstudio.com/)，二是[R语言IDE（RStudio）](http://www.rstudio.com/)。

安装步骤不再描述，基本上就是加入环境变量、关联相关文件和创建桌面图标。

![r-1-1.png](/asserts/imgs/r-1-1.png)

##2.向量

R语言中的向量就是一组值的列表

{% highlight R %}
#创建向量
> c(4, 7, 9)
[1] 4 7 9

#不同类型的对象创建向量，类型会被转换
> c(1, TRUE, "three")
[1] "1"     "TRUE"  "three"

#顺序数值向量
> 1:13
 [1]  1  2  3  4  5  6  7  8  9 10 11 12 13
 
#向量的访问
> sentence <- c('walk', 'the', 'plank')
> sentence[3]
[1] "plank"

#向量的名字
> ranks <- 1:3
> names(ranks)
> names(ranks) <- c("第一","第二","第三")
> ranks
第一 第二 第三 
 1    2    3

#绘制图形
> barplot(ranks)

{% endhighlight %}

绘制向量图形1，可见用R语言来画图非常方便！

![r-2-1.png](/asserts/imgs/r-2-1.png)

##3.矩阵
