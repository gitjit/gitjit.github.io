---
layout: post
title:  "Spark : Maven-eclipse setup"
date:   2015-12-04 7:40:22
categories: Spark
author : Jithesh Chandrasekharan
image: 
comments: true
meta: In this post we will set up spark eclipse project using Maven
---

In this post we will discuss how to set up a spark development environment using Maven and Eclipse-IDE for Scala.The application we are writing is a simple XML Parser. This setup will helps us to debug the application locally on a sample data set before running the application on a cluster. The IDE I am using is the eclipse IDE for Scala. You can download it from <a target="_blank" href = "http://scala-ide.org/">here</a>.

In the Eclipse IDE create a new Maven Project (File->New->Project->MavenProject). Then select "create a simple Project (skip archetype selection) " 

![Spark SBT](/img/maven-spark.png)

Before setting up Spark dependencies in pom.xml ,check your spark cluster and ensure that dependencies you specify in your pom.xml matches the cluster. One of the major issues I have seen in my experience is cross compilation issues between the Scala version that spark uses and Scala version that comes with Scala-IDE. To ensure that let us first check the spark version and scala version it uses.

![Spark SBT](/img/spark-version.png)

As shown above, Spark version installed is 1.5.2 and scala version it uses is 2.10.4 . So we need to ensure that the versions of dependencies in our pom.xml matches or should be a version below this since its all backward compatible. Now with this information let us configure our pom.xml.

**Dependencies**
{% highlight xml %}
<dependencies>
	<dependency>
		<groupId>org.apache.spark</groupId>
		<artifactId>spark-core_2.10</artifactId>
		<version>1.4.0</version>
	</dependency>
	<dependency>
		<groupId>org.apache.hadoop</groupId>
		<artifactId>hadoop-streaming</artifactId>
		<version>2.6.0</version>
	</dependency>
</dependencies>
{% endhighlight %}

Next thing we need is plugins for maven-scala integration and fat jar (uber jar). Please see the complete pom.xml <a target="_blank" href = "/assets/spark-maven-pom.xml">here</a>. Include the plugins to the pom.xml and save.Once you save the pom.xml these dependencies gets downloaded and you might find a project-out of date error. Just right click and do a quick fix for solving that issue.Then right click project->Configure->Add Scala-nature. This will create few errors and the reason is that Spark uses Scala 2.10 and Eclipse IDE comes with Scala 2.11. In order to fix this go to properties->Scala Compiler and select Scala Installation to 2.10.5 built-in as shown below.

![Spark SBT](/img/scala-compiler.png)

Next step is to remove Scala Library container from build path,since Spark-Core by default comes with Scala Library and we don't want to duplicate that.  (Project->ConfigureBuildPath->Libraries) and remove Scala Library Container. Next step is to change source folder to scala from Java. So just refactor and rename it to scala from java in both main and test source folder. 

That's it and now we are ready to write our Spark application using Scala and Eclipse IDE. Let us do that in our next post.


