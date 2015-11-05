---
layout: post
title:  "Spark : Programming Model"
date:   2015-10-22 7:40:22
categories: Spark
author : Jithesh Chandrasekharan
image: spark-prog-model.png
comments: true
---


Every Spark application consists of a driver program that contains your application's main function, defines the distributed datasets on the cluster and then applies operations on that datasets. When you run your program directly writing code in the interactive shell, the driver program is Spark Shell itself.Driver program access Spark through a SparkContext object. A SparkContext represents a connection to a computing cluster. In our example below we have created a Spark Context. But when you are directly writing on interactive shell, a spark context gets created automatically for you as the variable called sc.


**A simple Spark Program**

{% highlight python %}

from pyspark import SparkConf , SparkContext
import collections

conf = SparkConf().setMaster("local").setAppName("pylinefinder")
sc = SparkContext(conf = conf)

lines = sc.textFile("README.md")
py_lines = lines.filter(lambda line: "Python" in line)
print py_lines.count()

{% endhighlight %}

Executing:

{% highlight python %}

C:\spark>spark-submit linecount.py

output: 3

{% endhighlight %}


Once you have a SparkContext, you can use it to build RDDs. In example , we called sc.textFile() to create an RDD representing the lines of text in a file. We can then run various operations on these lines, such as count(). To run these operations, driver programs typically manage a number of nodes called executors. For example, if we were running the count() operation on a cluster, different machines might count lines in different ranges of the file. Because we just ran the Spark shell locally, it executed all its work on a single machine—but you can connect the same shell to a cluster to analyze data in parallel(picture above).

Spark API's revolves around passing functions to its operator to run them on cluster. In our example above we passed a function to the filter method as a lambda. Spark will take the function you specify and ships it to the executor nodes and executes there and final results  from all executors gets complied in the driver program which returns the results.





