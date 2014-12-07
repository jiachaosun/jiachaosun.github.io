---
layout: post
title: Mahout Clustering 聚类
categories: hadoop mahout
---

Mahout中有多种聚类算法实现，如Kmeans、fuzzy Kmeans、canopy等。

### 一、输入数据

mahout中聚类算法的输入格式是有要求的，需要一种称为SequenceFile格式的二进制文件。

这是一种Hadoop文件格式，详细的说明在 http://hadoop.apache.org/docs/current/api/org/apache/hadoop/io/SequenceFile.html 。

创建、读取和写入SequenceFile非常方便，使用Hadoop相关的IO API即可（SequenceFile.Writer, SequenceFile.Reader and SequenceFile.Sorter）。

我们根据MIA中的例子来看下，很简单的x,y坐标点数据二维数组，第一步是讲输入数据变成vector格式的向量。

{% highlight java %}
double[][] points = { { 1, 1 }, { 2, 1 }, { 1, 2 }, { 2, 2 }, { 3, 3 }, { 8, 8 }, { 9, 8 }, { 8, 9 }, { 9, 9 } };

 public static List<Vector> getPoints(double[][] raw) {
  List<Vector> points = new ArrayList<Vector>();
  for (int i = 0; i < raw.length; i++) {
   double[] fr = raw[i];
   Vector vec = new RandomAccessSparseVector(fr.length);
   vec.assign(fr);
   points.add(vec);
  }
  return points;
 }

{% endhighlight %}

points是List<Vector>，它们才是稍后写入SequenceFile的真正输入。

{% highlight java %}
  public static void writePointsToFile(List<Vector> points, String fileName, FileSystem fs, Configuration conf)
    throws IOException {
   Path path = new Path(fileName);
   SequenceFile.Writer writer = new SequenceFile.Writer(fs, conf, path, LongWritable.class, VectorWritable.class);
   long recNum = 0;
   VectorWritable vec = new VectorWritable();
   for (Vector point : points) {
    vec.set(point);
    writer.append(new LongWritable(recNum++), vec);
   }
   writer.close();
  }
{% endhighlight %}

也就是K的值。MIA中是随便选了2个点（points中前2个点），从结果中可以看到，即使不能准确判断init point的值，Kmeans算法会在计算过程中自我不断调整，最终达到一个比较理想的结果。

（代码省略）

### 运行算法

有了第一第二部最主要的输入数据，就可以运行Kmeans算法了，调用方法为
{% highlight java %}
  // run kmeans.
  KMeansDriver.run(conf, new Path("testdata/points"), new Path("testdata/clusters"), new Path("output"),
    new EuclideanDistanceMeasure(), 0.001, 10, true, false);
{% endhighlight %}

聚类结果会输出到output目录下的 Cluster.CLUSTERED_POINTS_DIR 这个常量值作为名称的目录中。

就如最开始说的，结果也是一个SequenceFile文件，我们可以通过SequenceFile.Reader来读取。

{% highlight ruby %}
  SequenceFile.Reader reader = new SequenceFile.Reader(fs, new Path("output/" + Cluster.CLUSTERED_POINTS_DIR
    + "/part-m-00000"), conf);

  IntWritable key = new IntWritable();
  WeightedVectorWritable value = new WeightedVectorWritable();
  while (reader.next(key, value)) {
   System.out.println(value.toString() + " belongs to cluster " + key.toString());
  }
{% endhighlight %}

至此，一个最简单的Mahout聚类算法就运行完了。可以看出，Mahout帮助我们完成了很大一部分工作，真正花时间的并不是写这些代码，而是数据预处理和后期调优。