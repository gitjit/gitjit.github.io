---
layout: post
title:  "Spark : sbt-eclipse setup"
date:   2015-12-04 7:40:22
categories: Spark
author : Jithesh Chandrasekharan
image: 
comments: true
meta: In this post we will set up spark eclipse project using sbt.
---

In this post we will discuss how to set up a spark application using SBT and Eclipse-IDE for Scala.The application we are writing is a simple WordCounter. SBT is an interactive build tool for Scala just like Maven/Ant.SBT has native support for Scala code compilation , test frame work integration and dependency management using IVY.

Ensure that <a target="_blank" href = "http://www.scala-sbt.org/">SBT</a>, Scala and <a target="_blank" href = "http://scala-ide.org/">Scala-IDE</a> is installed on your system. As a first step create a root folder for your project in your workspace . In my case the project name is Sparks. Then create a build.sbt file in the project root folder and add following entries into it. (The entries might vary based on versions you installed)

{% highlight shell %}
> mkdir Sparks
> cd Sparks
> touch build.sbt
> vim build.sbt 
{% endhighlight %}

![Spark SBT](/img/sbt.png)

Now let us create another folder named "project" (Sparks/project) inside the Sparks folder and add two files build.properties and plugins.sbt with following informations. 

Add SBT version information.
![Spark SBT](/img/sbt-build.png)
Add eclipse plugin information.
![Spark SBT](/img/sbt-plugin.png)

Now run the following command from root folder of your project to create a Scala Project which we can import into Eclipse. This command will generate necessary folder structure for us to use SBT for continuous compilation and packaging.

{% highlight shell %}
jmac:sparks jit$ sbt eclipse
{% endhighlight %}

Now import the generated project into Eclipse. (File->Import->ExistingProjectIntoWorkSpace). Then add following code for word count application.

{% highlight scala %}
package main

import org.apache.spark.SparkContext
import org.apache.spark.SparkConf
import java.io.File

object WordCounter {
  
  def main(args:Array[String])
  {
    //val current = new File(".").getAbsolutePath
    //println(current)
    val conf = new SparkConf().setAppName("WordCounter")
    val sc = new SparkContext(conf)
    val txtFile = sc.textFile("data/wc.txt")
    val words = txtFile.flatMap(line => line.split(" "))
    val wordCnt = words.map(word => (word,1))
    val wordReduced = wordCnt.reduceByKey((acc,newVal) => acc+newVal)
    val sortedCnt = wordReduced.sortBy(kv => kv._2, false)
    sortedCnt.saveAsTextFile("output/wcount.txt")
    
  }
  
}
{% endhighlight %}

Next step is to compile and package to a jar file which can be run on a Spark cluster.

{% highlight scala %}
jmac:sparks jit$ sbt package
// result [info] Packaging ../target/scala-2.11/sparks_2.11-0.1.jar ...
{% endhighlight %}

Now submit the spark job.
{% highlight shell %}
jmac:sparks jit$ spark-submit --class "main.WordCounter" --master 
"local[*]" target/scala-2.11/sparks_2.11-0.1.jar
{% endhighlight %}
spark-submit is the command to submit a job. --class specifies the main entry point of the application and --master specifies the master node and in our case its local and * specifies to utilize all available cores.
