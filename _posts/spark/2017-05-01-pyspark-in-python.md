---
layout: post
title:  "Pyspark and Jupyter notebook setup in Ubuntu"
description: "Setting up pyspark in Ubuntu linux"
date:   2017-05-01
tags: [Spark, DataEngineer]
comments: true
references: [
   "Apache Spark : https://spark.apache.org/",
   "Apache Spark Documentation: https://spark.apache.org/docs/latest/",
]

excerpt: "This post is about setting up pyspark in Ubuntu Linux. For python programmers interested in doing data engineering, pyspark is a very good option.
We will also set up Jupyter notebook, which enables us to use notebooks to write Spark programs."
---

As a first step ensure that Python3 is installed in Ubuntu. By default latest version of Ubuntu comes with Python2 and Python3 versions. 

Step 1 : Install PIP3

```bash
jit@ubuntu:~$ sudo apt install python3-pip

```

Step 2 : Install Jupyter Notebook

```bash
jit@ubuntu:~$ pip3 install jupyter

```

Step 3 : Install Java / update java runtime.

```bash
jit@ubuntu:~$ sudo apt-get update
jit@ubuntu:~$ sudo apt-get install default-jre
jit@ubuntu:~$ java -version # check java version

```

Step 4 : Install Scala   

```bash
jit@ubuntu:~$ sudo apt-get install scala  
jit@ubuntu:~$ scala -version # check scala version
```

Step 5 : Install Py4J(It enables python programs running in a python interpreter to dynamically access Java objects in JVM)

`jit@ubuntu:~$ pip3 install py4j`

Step 6: Download Spark from <http://spark.apache.org/downloads.html>   

![](..\..\images\2017-05-24-21-59-43.png)  

Step 7 : unzip the tar and move to home folder 

```bash 
jit@ubuntu:~/Downloads$ sudo tar -zxvf spark-2.1.1-bin-hadoop2.7.tgz 
```

Step 8 : Configure Spark and Jupyter environment variables in .profile  and source it 

```bash
PATH="$HOME/bin:$HOME/.local/bin:$PATH"
export SPARK_HOME='~/spark-2.1.1-bin-hadoop2.7'
export PATH=$SPARK_HOME:$PATH
export PYTHONPATH=$SPARK_HOME/python:$PYTHONPATH
export PYSPARK_DRIVER_PYTHON="jupyter"
export PYSPARK_DRIVER_PYTHON_OPTS="notebook"
export PYSPARK_PYTHON=python3

```

Step 9 : Set permissions on spark folders  

```python
jit@ubuntu:~$ sudo chmod 777 spark-2.1.1-bin-hadoop2.7
              sudo chmod 777 python
              sudo chmod 777 python/pyspark
```

Instead of adding pyspark folders to path, let us use another module called `findspark`.  

Step 10 : Install findspark  

`jit@ubuntu:~$ pip3 install findspark`  

Now our installation is complete and try following steps in a Jupyter notebook.  

```python
import findspark
findspark.init('/home/jit/spark-2.1.1-bin-hadoop2.7')
import pyspark
```

If no errors our Pyspark and Jupyter notebook set up is successful. 
