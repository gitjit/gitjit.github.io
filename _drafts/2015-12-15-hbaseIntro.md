---
layout: post
title:  "HBase Tutor"
date:   2015-10-29 09:45:36
categories: spark hadoop
author : Jithesh Chandrasekharan
---



HBase is an open source, non-relational, distributed database modeled after Google's BigTable and written in Java. It is developed as part of Apache Software Foundation's Apache Hadoop project and runs on top of HDFS (Hadoop Distributed Filesystem), providing BigTable-like capabilities for Hadoop. That is, it provides a fault-tolerant way of storing large quantities of sparse data (small amounts of information caught within a large collection of empty or unimportant data, such as finding the 50 largest items in a group of 2 billion records, or finding the non-zero items representing less than 0.1% of a huge collection).

HBase features compression, in-memory operation, and Bloom filters on a per-column basis as outlined in the original BigTable paper.[1] Tables in HBase can serve as the input and output for MapReduce jobs run in Hadoop, and may be accessed through the Java API but also through REST, Avro or Thrift gateway APIs.

HBase is not a direct replacement for a classic SQL database, however Apache Phoenix project provides a SQL layer for Hbase as well as JDBC driver that can be integrated with various analytics and business intelligence applications.

Hbase is now serving several data-driven websites,[2][3] including Facebook's Messaging Platform.[4][5]

In the parlance of Eric Brewer’s CAP theorem, HBase is a CP type system.