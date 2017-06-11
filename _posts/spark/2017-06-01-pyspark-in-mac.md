---
layout: post
title:  "Pyspark and Jupyter notebook setup in Mac"
description: "Setting up pyspark in Mac OS"
date:   2017-06-01
tags: [Spark]
comments: true
references: [
   "Apache Spark : https://spark.apache.org/",
   "Apache Spark Documentation: https://spark.apache.org/docs/latest/",
]

excerpt: "This post is about setting up pyspark in Mac OS. For python programmers interested in doing data engineering, pyspark is a very good option. We will also set up Jupyter notebook, which enables us to use notebooks to write Spark programs."
---  

Step 1: 

Install latest Python3 in Mac OS (If you already have Python3 that should work perfectly fine too). I prefer Anaconda distribution since it comes with lot of packages which we need in further development. You can install Anaconda distribution from [here](https://www.continuum.io/downloads#macos).

```bash
jmac:~ jit$ python
Python 3.6.1 |Anaconda 4.4.0 (x86_64)| (default, May 11 2017, 13:04:09) 
[GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.57)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 

```
Step 2:

Install latest Java in your Mac. I am downloading the SDK from here, but latest Java runtime should do the job. You
can download and install Java from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).  

```bash
jmac:~ jit$ java -version
java version "1.8.0_131"
Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)
jmac:~ jit$ 

```
Step 3:

Download Apache Spark from [here](http://spark.apache.org/downloads.html) to a preferred folder. I downloaded
to a Dev folder in my home directory. 

![](..\..\images\2017-05-24-21-59-43.png)  

Now unzip the tar.  

`jmac:Dev jit$tar -xzf spark-2.1.1-bin-hadoop2.7.tgz`  

Step 4 : 

Now add anaconda and  spark binaries to path. 

```bash
# added by Anaconda3 4.4.0 installer
export PATH="/Users/jit/Dev/anaconda/bin:$PATH"
export SPARK_HOME="/Users/jit/Dev/spark-2.1.1-bin-hadoop2.7"
export PATH=$SPARK_HOME/bin:$PATH
``` 

Step 5: 
Check the installation.  

```bash
jmac:~ jit$ pyspark
Python 3.6.1 |Anaconda 4.4.0 (x86_64)| (default, May 11 2017, 13:04:09) 
[GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.57)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
17/06/11 14:58:09 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
17/06/11 14:58:24 WARN ObjectStore: Failed to get database global_temp, returning NoSuchObjectException
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 2.1.1
      /_/

Using Python version 3.6.1 (default, May 11 2017 13:04:09)
SparkSession available as 'spark'.
>>> 

```
step 6: 

Now let us configure the Jupyter notebook for developing PySpark applications. Easiest way to do this 
is by installing findspark package.  

`jmac:~ jit$ pip install findspark`

Now open Jupyter notebook and let us try a simple pyspark application.  

<img src='/images/2017-06-11-15-07-34.png' class='img-responsive'>"

_Programming is fun . Enjoy !_