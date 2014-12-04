---
layout: post
title: mahout 源码编译+适配Hadoop 2.x
categories: mahout
---

最近工作中用到一些数据挖掘和机器学习的技术，涉及推荐系统、聚类算法和分类算法。

所以打算好好把Mahout学习的过程记录下来，一来方便以后查阅，二来也分享自己的一些心得体会。

### 一 mahout源码编译

官网 http://mahout.apache.org/ 可以下载到编译后的版本（最新0.9），不过并不兼容最新的hadoop 2.X，所以我需要自己编译。

从github克隆代码后，使用maven命令编译打包。

{% highlight bash %}
git clone https://github.com/apache/mahout.git
{% endhighlight %}

会下载大量的依赖jar包，截止目前github上mahout仓库中克隆下来的代码并不支持JDK 1.8，编译会报错，使用JDK 1.7就没有问题了。

不过还有一个窍门，如果不需要mahout-maths或者mahout-scala相关的包，可以在pom.xml文件中查找module元素，去除掉相关module即可，这样可以节约大量时间。

![123](/asserts/imgs/mahout/m-1-1.png)

### 二 适配 Hadoop 2.x

Mahout目前最新版本是0.9，官方预编译的版本中只兼容了Hadoop 1.X，可以从pom.xml文件中的hadoop版本看到。

![123](/asserts/imgs/mahout/1/20141204160702531.png)

为了让mahout在hadoop 2.X上运行，你必须从git上clone最新的master代码并且自行编译。

![123](/asserts/imgs/mahout/1/20141204160734677.png)

编译命令如下：

{% highlight bash %}
mvn -DskipTests=true clean package
{% endhighlight %}

泡杯咖啡回来，终于出现了全部编译打包成功的画面。

![123](/asserts/imgs/mahout/m-1-2.png)

之后再运行就没问题了。

{% highlight bash %}
mahout recommenditembased -s SIMILARITY_LOGLIKELIHOOD -i \
/path/to/input/file -o /path/to/desired/output --numRecommendations 25
{% endhighlight %}

![123](/asserts/imgs/mahout/1/20141204160821734.png)

可以从命令行中看到，mahout已经运行了分布式的推荐算法。