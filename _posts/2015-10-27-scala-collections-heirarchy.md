---
layout: post
title:  "Scala : Collections Heirarchy"
date:   2015-10-27 7:40:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
---

One of the main attraction of Scala is that we can use the vast collection of Java Libraries in Scala. In addition to that Scala has its own collection libraries. Ability to store data efficiently and do manipulations on that data makes a programming language powerful. In this post we will discuss some of the collection libraries provided by Scala.

Scala collections are part of the main package "scala.collection._" . Also collections are split into two diffrent packages based on mutable and immutable as scala.collection.mutable and scala.collection.immutable. Also have a parallel collection library under scala.collection.parallel. But all scala collections follows a type heirarchy as shown below.

![Scala Type Heirarchy](/img/scala_col.png)

As shown above all collections are primarily having two traits where traversable means we can reach each elements in it at least once and iterable means it provides an iterator for iterations. Then there are 3 main abstract types (set,seq and map). set is a type that won't allow duplicates, seq allows duplicates but provides an ordering and map allows to look up based on key. As name implies indexed seq allows random lookup based on index, while linear seq requires linear time for look up.  Sets are by default non sorted but we have a sorted set available too. All these are abstract types and their concrete implementations are in scala.collection.mutable or scala.collection.immutable package. The figure below shows the concrete implementation on both mutable and immutable packages.

**Immutable**

![Scala Type Heirarchy](/img/immutable.png)

**Mutable**

![Scala Type Heirarchy](/img/mutable.png)





