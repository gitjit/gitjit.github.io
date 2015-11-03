---
layout: post
title:  "Apache Spark/IPythonNB Setup"
date:   2015-10-20 7:40:22
categories: Apache Spark
author : Jithesh Chandrasekharan
image: 
comments: true
---

Apache Spark is an open source cluster computing framework originally developed in the AMPLab at University of California, Berkeley but was later donated to the Apache Software Foundation where it remains today. In contrast to Hadoop's two-stage disk-based MapReduce paradigm, Spark's multi-stage in-memory primitives provides performance up to 100 times faster for certain applications. By allowing user programs to load data into a cluster's memory and query it repeatedly, Spark is well-suited to machine learning algorithms.

**Setting up Spark in Windows**

Following are the steps to set up Apache Spark running in your Windows desktop. Here I am using Windows10 Professional. 

Step 1: Install a Python distribution.
   : I am using Anaconda from continuum analytics, Python 2.7. Download it from <a target="_blank" href="https://www.continuum.io/downloads">here</a> 
  
Step 2: Install latest Java Development Kit. 
   : You can download it from <a target="_blank" href = "http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html">here</a>

Step 3: Install Apache Spark.
   : Here I am using a version prebuilt for Hadoop 2.6 instead of building it from the source. Download it from <a target="_blank" href="http://spark.apache.org/downloads.html">here</a>

**Download the tar package.**

![Spark Download](/img/spark-install.png)
After downloading, un-tar it to a home folder for spark. Then go to the conf folder and change the log4j settings, that will be really helful in avoiding unwanted messages in your terminal. (log4j.rootCategory=WARN, console). After that rename the file to log4j.properties.

**Add following environment Variables(User)**

SPARK_HOME: (This points to location where Spark binaries are available). Then add this variable to path.

HADOOP_HOME: Spark expects to find hadoop for windows, this is a minor glitch which can be avoided by providing a path to a folder containing 'winutils.exe' and set it as HADOOP_HOME.You can download it from  <a target="_blank" href = "http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe">here</a> . After that create a folder called winutils\bin and paste this binary there. 

{% highlight python %}
SPARK_HOME = C:\spark
PATH = %PATH%;%SPARK_HOME%\bin 
HADOOP_HOME = C:\winutils
{% endhighlight %}

Now we ar ready to run spark. So now go to spark home folder and start a terminal/console there and type 'pyspark'. You should find spark running.In order to ensure,let us run a simple spark program that creates an RDD from the readme file in the spark home and count number of lines.

![Spark Download](/img/spark-install1.png)
If everything goes fine, you will see the line number, in my case its 98.

**Setting up IPython Notebook**
There are multiple ways you can do this, one method is creating IPython Notebook pyspark profile and other way to do this is by importing "findspark" module.I am going with second method.

{% highlight python %}
$ pip install findspark
$ IPython Notebook
{% endhighlight %}

![Spark Notebook](/img/spark-install2.png)






