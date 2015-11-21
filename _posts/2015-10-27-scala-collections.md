---
layout: post
title:  "Scala : Collections"
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

Now let us discuss some of the most useful sequences.

**Arrays and List**
One of the most frequently used collection types in Scala are Array and List. Array is mutable, while Lists are immutable in Scala. Eventhough its not a good practice, but you can have heterogeneous types, but scala will do a type inference to common type. Its important to note that in Scala we access elements using () instead of [] in other languages.

{% highlight scala %}

  val arr = Array(1,2,3,4)         //>Array[Int] = Array(1, 2, 3, 4)
  val arr1 = Array(1,2,"hello",4)   //>Array[Any] = Array(1, 2, hello, 4)
  val arr2 = new Array[Int](5)      //>Array[Int] = Array(0, 0, 0, 0, 0)
  arr2(1) = 10
  arr2(2) = 20
  val result = arr2(1)              //> result  : Int = 10

  scala> val fiveInts = new Array[Int](5)
  //fiveInts: Array[Int] = Array(0, 0, 0, 0, 0)

  // for larger arrays
  val arr = Array.fill(10)(1)                       
  //> arr  : Array[Int] = Array(1, 1, 1, 1, 1, 1, 1, 1, 1, 1)
  val arr1 = Array.tabulate(10)(i => i*i) // index based filling.
  //arr1 : Array(0, 1, 4, 9, 16, 25, 36, 49, 64, 81)
  

{% endhighlight %}

A list in Scala is immutable and is internally implemented as singly immutable linked list.

{% highlight scala %}
val mainList = List(3, 2, 1) //List[Int] = List(3, 2, 1)
val with4 = 4::mainList      //List[Int] = List(4, 3, 2, 1)
val with43 = 43::mainList   //with43  : List[Int] = List(43, 3, 2, 1)
mainList         //List[Int] = List(3, 2, 1)

scala> val colors = List("red", "blue", "green")
//colors: List[java.lang.String] = List(red, blue, green)
scala> colors.head
//res0: java.lang.String = red
scala> colors.tail
//res1: List[java.lang.String] = List(blue, green)

{% endhighlight %}

**Vector**
Vectors  in Scala can be considered as a combination of Array and List. That is its an immutable indexed collection. (Array you can index but mutable, where list is immutable but cannot be indexed.) . Vector is implemented as a multiway tree that helps to improve caching so that each node in tree fits nicely on cache line and that helps to find an item in collection very quickly. Other than that everything else applicable for array and list fits nicely into vectors too.

{% highlight scala %}

Vector(1,2,3)

{% endhighlight %}

**Buffer**

There are two types ArrayBuffer and ListBuffer . ArrayBuffer is commonly used and it can grow easily and indexible. Since buffers are highly mutable it lies in a package scala.collections.mutable. (By default everything under Scala is imported so it can also be considered as collections.mutable). By default collections.immutable is imported, so its a best practice to use full name for mutable types. ArrayBuffer is quickly indexible.

Never import "scala.collections.mutable._" (since there are classes like Set and Map in both packages). So the best practice when using mutable is like this.

{% highlight scala %}
import scala.collections.mutable
mutable.buffer(10,20,30) // ArrayBuffer default
{% endhighlight %}


{% highlight scala %}

scala> import scala.collection.mutable.ListBuffer
  import scala.collection.mutable.ListBuffer

scala> val buf = new ListBuffer[Int]             
  buf: scala.collection.mutable.ListBuffer[Int] = ListBuffer()

scala> buf += 1                                  

scala> buf += 2                                  

scala> buf     
  res11: scala.collection.mutable.ListBuffer[Int]
    = ListBuffer(1, 2)

scala> 3 +: buf                                  
  res12: scala.collection.mutable.Buffer[Int]
    = ListBuffer(3, 1, 2)

scala> buf.toList
  res13: List[Int] = List(3, 1, 2)

{% endhighlight %}

{% highlight scala %}

scala> import scala.collection.mutable.ArrayBuffer
  import scala.collection.mutable.ArrayBuffer

scala> val buf = new ArrayBuffer[Int]()
  buf: scala.collection.mutable.ArrayBuffer[Int] = 
    ArrayBuffer()
scala> buf += 12

scala> buf += 15

scala> buf
  res16: scala.collection.mutable.ArrayBuffer[Int] = 
    ArrayBuffer(12, 15)

scala> buf.length
  res17: Int = 2

scala> buf(0)
  res18: Int = 12

{% endhighlight %}

**Range**

Range represents a sequence of values having a nice step to them 

{% highlight scala %}
val oneto10 = 1 to 5                              
//> oneto10  : Range(1, 2, 3, 4, 5)
val oneUntil10 = 1 until 10                      
 //> oneUntil10  :  Range(1, 2, 3, 4, 5, 6, 7, 8,9)
val odd = 1 to 10 by 2                            
//> odd  : Range(1, 3, 5, 7, 9)

val flt = 0.1 to 0.5 by 0.1                       
//> flt  : NumericRange[Double] = NumericRange(0.1, 0.2
                //|, 0.30000000000000004, 0.4, 0.5)

val back = 5 to 1 by -1                          
 //> back  : immutable.Range = Range(5, 4, 3, 2, 1)
                                           
val b = 5.to(1).by(-1)                            
//> b  : immutable.Range = Range(5, 4, 3, 2, 1)

{% endhighlight %}

In Scala everything is objects, so when you specify 1+2 (internall its (1).+(2) , where '+' is a function in Int object and in a similar way as shown above for rangest 5 to 1 = (5).to(1). Its a rule in scala that if you have a function with 1 or 0 arguments you can use it with infix notation omitting the . and ().



