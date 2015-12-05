---
layout: post
title:  "Spark : EcoSystem"
date:   2015-12-01 7:40:22
categories: Spark
author : Jithesh Chandrasekharan
image: spark-echosystem.png
comments: true 
---

**What/Why Apache Spark?:**
Apache Spark is a cluster computing framework designed for speed and general purpose computing. Speed plays an important part in exploring large datasets, that is reducing the wait time of batch processing and doing things more interactively. One of the main attraction of Spark is its ability to run computation in memory, but obviously large data sets cannot be completely computed in memory, there it supports running complex applications on disk and yet proven much efficient than MapReduce.

Another advantage of Spark is the concept of a single engine to cover a wide range of workloads that previously required separate distributed systems, including batch applications, interactive queries and streaming. So by supporting all these workloads in the same engine, Spark makes it easy to combine different processing types in production pipelines and also up to a large extend reduces the burden of managing separated tools.

Spark is highly scalable and offers simple API in languages like Python,Java,Scala,SQL,R,Clojure and more to come. 

**Hadoop and Spark:**
Hadoop as a big data processing technology has been around for 10 years and has proven to be the solution of choice for processing large data sets. MapReduce is a great solution for one-pass computations, but not very efficient for use cases that require multi-pass computations and algorithms. Each step in the data processing work flow has one Map phase and one Reduce phase and you'll need to convert any use case into MapReduce pattern to leverage this solution.

The Job output data between each step has to be stored in the distributed file system before the next step can begin. Hence, this approach tends to be slow due to replication & disk storage. Also, Hadoop solutions typically include clusters that are hard to set up and manage. It also requires the integration of several tools for different big data use cases (like Mahout for Machine Learning and Storm for streaming data processing).

If you wanted to do something complicated, you would have to string together a series of MapReduce jobs and execute them in sequence. Each of those jobs was high-latency, and none could start until the previous job had finished completely.

Spark allows programmers to develop complex, multi-step data pipelines using directed acyclic graph (DAG) pattern for lazy evaluation. It also supports in-memory data sharing across DAGs, so that different jobs can work with the same data.

Spark runs on top of existing Hadoop Distributed File System (HDFS) infrastructure to provide enhanced and additional functionality. It provides support for deploying Spark applications in an existing Hadoop v1 cluster (with SIMR – Spark-Inside-MapReduce) or Hadoop v2 YARN cluster or even Apache Mesos.

Spark is considered as an alternative to Hadoop MapReduce rather than a replacement to Hadoop. It’s not intended to replace Hadoop but to provide a comprehensive and unified solution to manage different big data use cases and requirements.Spark can create distributed datasets from any file stored in the Hadoop distributed filesystem (HDFS) or other storage systems supported by the Hadoop APIs (including your local filesystem, Amazon S3, Cassandra, Hive, HBase, etc.). It’s important to remember that Spark does not require Hadoop; it simply has support for storage systems implementing the Hadoop APIs. Spark supports text files, SequenceFiles, Avro, Parquet, and any other Hadoop InputFormat. 

**Spark Unified Stack:**
The image above shows the spark's unified stack. All components are closely integrated to spark-core, which is the computational engine responsible for scheduling, distributing and monitoring applications consisting of multiple computational tasks that spawn across multiple worker machines in the cluster.In case of hadoop, we will have to use seperate frameworks based on our needs. For example Storm for stream processing, hive for querying, scalading for reducing mapreduce complexity, mahout for maching learning etc.. But Sparks tries to unify them together under a single stack.

![Spark Download](/img/spark-hadoop.png)

Since the spark-core is general purpose and fast, multiple higher level components can be build top on that specialized for various workloads. The components in spark stack is closely integrated with spark-core so that they can inter-operate closely. This tight integration allows benefits such as improvement in lower layer will get easily utilized to upper layer (Eg: Bug fix/speed optimization in spark-core).Single software system hence easy maintainable and a new component added to spark stack can be easily available with out much effort. 

The biggest advantage of this model is  the ability to build applications that seamlessly combine different processing models. For example, in Spark you can write one application that uses machine learning to classify data in real time as it is ingested from streaming sources. Simultaneously, analysts can query the resulting data, also in real time, via SQL (e.g., to join the data with unstructured logfiles). In addition, more sophisticated data engineers and data scientists can access the same data via the Python shell for ad hoc analysis. Others might access the data in standalone batch applications. All the while, the IT team has to maintain only one system.

**Spark's Components:**

Here we will briefly discuss each of the spark components (as shown in the picture above).

**Spark Core:**
Spark-core as its name suggests is the core of the spark stack that binds all components.Its responsible for all basic spark functionalities including memory management, fault-recovery, storage system interaction and more. One of the major programming abstraction of spark is called as RDD(Resilient Distributed Dataset) which is defined in spark-core. RDD is a collection of items distributed across multiple nodes and can be manipulated in parallel.Spark-core contains lot of API's to create,transform/manipulate these collections. 

**Spark SQL:**
Spark SQL is Spark’s package for working with structured data. It allows querying data via SQL as well the Hive Query Language (HQL)—and it supports many sources of data, including Hive tables, Parquet, and JSON. In addition to provide an SQL interface it allows developers to write programs that manipulate RDD's in other languages(python,java,scala) along with SQL queries and thus allowing combining SQL with complex analytics.
Note : Shark (Berkeley) was an older project that modified Apache Hive to run on Spark. It has now been replaced by Spark SQL to provide better integration with the Spark engine and language APIs.

**Spark Streaming:**
Spark Streaming is the component that allows processing of live streams of data (twitter streams, server logs, other messages queues containing regular updates..)The streaming API's extends RDD's API and hence easy transition based on source of data. 

**MLLib:**
MLLib as name suggests is spark's library that provides basic machine learning functionalities. MLlib provides machine learning algorithms like regressions,clustering,classifications, collaborative filtering etc.. All these are designed to scale across clusters.

**GraphX:**
GraphX is a library for manipulating graphs (e.g., a social network’s friend graph) and performing graph-parallel computations. Like Spark Streaming and Spark SQL, GraphX extends the Spark RDD API, allowing us to create a directed graph with arbitrary properties attached to each vertex and edge. GraphX also provides various operators for manipulating graphs (e.g., subgraph and mapVertices) and a library of common graph algorithms (e.g., PageRank and triangle counting).

**Cluster Managers:**
Under the hood, Spark is designed to efficiently scale up from one to many thousands of compute nodes. To achieve this while maximizing flexibility, Spark can run over a variety of cluster managers, including Hadoop YARN, Apache Mesos, and a simple cluster manager included in Spark itself called the Standalone Scheduler. If you are just installing Spark on an empty set of machines, the Standalone Scheduler provides an easy way to get started; if you already have a Hadoop YARN or Mesos cluster, however, Spark’s support for these cluster managers allows your applications to also run on them.

**Lines of Code - Comparison**
Lesser the code, easy management and optimization. The picture below shows code comparison (As of April-2015) on some of the major big data frameworks. As you can see Spark components are reusing Spark Core and hence any optimization in Core is reflecting on libraries built on top of that.

![Spark Download](/img/code-size.png)


