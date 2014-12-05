---
layout: post
title: Hadoop 2.x Yarn 配置优化
categories: hadoop yarn
---

目前集群中节点的配置是 16 cores + 32 GB 内存，使用默认配置的hadoop集群在运行一些大任务时候经常报错，错误内容各不相同。

经过一段时间的调优，发现首先要修改的是 nodemanager 最大内存.

yarn-site.xml

{% highlight xml %}
<name>yarn.nodemanager.resource.memory-mb</name>
<value>25000</value>
{% endhighlight %}


下一步是计算yarn如何分配资源到每个container，我们希望每个节点最多运行11个containers，所以用总内存/期望container数=每个container最小内存。

在我们集群上就是 32*0.8 / 10 = 2.5G ，为了稳定起见我们分配的值最要再小于这个计算值一些。

{% highlight xml %}
<name>yarn.scheduler.minimum-allocation-mb</name>
<value>2048</value>
{% endhighlight %}

接下来是配置mapreduce参数，有以下三方面需要考虑到。

1.每个map和reduce任务的物理内存限制。

2.JVM堆内存限制。

3.每个任务的虚拟内存限制。


由于每个map和reduce会运行在独立的container中，所以最大map和reduce内存至少需要大于等于yarn-site中的container最小内存限制。

mapred-site.xml

{% highlight xml %}
<name>mapreduce.map.memory.mb</name>
<value>3000</value>
<name>mapreduce.reduce.memory.mb</name>
<value>5000</value>

<name>mapreduce.map.java.opts</name>
<value>-Xmx2200m</value>
<name>mapreduce.reduce.java.opts</name>
<value>-Xmx3600m</value>
{% endhighlight %}

虚拟内存默认是物理内存的2.1倍，这个值使用默认即可。

{% highlight xml %}
<name>yarn.nodemanager.vmem-pmem-ratio</name>
<value>2.1</value>
{% endhighlight %}

经过以上参数，目前集群中每个map任务会使用到最多

1. 物理内存分配 = 3G

2. JVM堆内存 = 2.2G

3. 虚拟内存最大 = 6.2G

每个节点最大分配到 25/3 = 8个map和 5个reduce。