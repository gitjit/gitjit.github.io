---
layout: post
title:  "Scala : Collections"
date:   2015-10-29 7:50:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
---

**Arrays and List**
One of the most frequently used collection types in Scala are Array and List. Array is mutable, while Lists are immutable in Scala. Eventhough its not a good practice, but you can have heterogeneous types, but scala will do a type inference to common type. Its important to note that in Scala we access elements using () instead of [] in other languages. It is important that most of functions is defined in "Sequnce" and hence will be applicable to all Sequence subtypes.

**Array Operations**

---------------------------------
val arr1 = Array(1,2,3,4)                         //> arr1  : Array[Int] = Array(1, 2, 3, 4)
val arr2 = Array(5,6,7,8)                         //> arr2  : Array[Int] = Array(5, 6, 7, 8)

//++ to add two sequences
// creates a new array with out affecting originals
val arr3 = arr1 ++ arr2                           //> arr3  : Array[Int] = Array(1, 2, 3, 4, 5, 6, 7, 8)

//head and tail
arr1.head                                         //> res0: Int = 1
arr1.last                                         //> res1: Int = 4
arr1.tail                                         //> res2: Array[Int] = Array(2, 3, 4)

//drop and take

val arr = arr2.drop(2)                            //> arr  : Array[Int] = Array(7, 8)
arr                                               //> res3: Array[Int] = Array(7, 8)
arr2.take(3)                                      //> res4: Array[Int] = Array(5, 6, 7)
arr2.dropRight(2)                                 //> res5: Array[Int] = Array(5, 6)
arr2.reverse                                      //> res6: Array[Int] = Array(8, 7, 6, 5)

// split

val(bef,after) = arr3.splitAt(4)                  //> bef  : Array[Int] = Array(1, 2, 3, 4)
                                                  //| after  : Array[Int] = Array(5, 6, 7, 8)
                                                  
//slice
arr3.slice(3,7)                                   //> res7: Array[Int] = Array(4, 5, 6, 7)


//add to head/tail
val arr4 = Array.fill(10)(0)                      //> arr4  : Array[Int] = Array(0, 0, 0, 0, 0, 0, 0, 0, 0, 0)
arr3 :+ 2                                         //> res8: Array[Int] = Array(1, 2, 3, 4, 5, 6, 7, 8, 2)
2 +: arr3                                         //> res9: Array[Int] = Array(2, 1, 2, 3, 4, 5, 6, 7, 8)

':' position defines on which type its operated. So arr3 :+ 2 = arr3.+(2) and
 2 +: arr3 = arr3.+(2)


arr3.size                                         //> res10: Int = 8
arr3.length                                       //> res11: Int = 8

//empty startsWith
arr3.startsWith(Array(1,2))                       //> res12: Boolean = true
arr3.endsWith(Array(3,4))                         //> res13: Boolean = false
arr1.isEmpty                                      //> res14: Boolean = false

//handy
arr3.max                                          //> res15: Int = 8
arr3.min                                          //> res16: Int = 1
arr3.sum                                          //> res17: Int = 36
arr3.product                                      //> res18: Int = 4032

// Other Generic	
val arr5 = Array(1,2,3,4)         //>Array[Int] = Array(1, 2, 3, 4)
val arr6 = Array(1,2,"hello",4)   //>Array[Any] = Array(1, 2, hello, 4)
val arr2 = new Array[Int](5)      //>Array[Int] = Array(0, 0, 0, 0, 0)
arr2(1) = 10
arr2(2) = 20
val result = arr2(1)              //> result  : Int = 10

//Tabulate for index based filling
val arr1 = Array.tabulate(10)(i => i*i) // index based filling.
//arr1 : Array(0, 1, 4, 9, 16, 25, 36, 49, 64, 81)

//set operations
arr1.intersect(arr3)                              //> res19: Array[Int] = Array(1, 2, 3, 4)
arr1.union(arr3)                                  //> res20: Array[Int] = Array(1, 2, 3, 4, 1, 2, 3, 4, 5, 6, 7, 8)

Note : Most of the operations create a new array and might be good to use in immutables like List.

-------------------------------

Scala doesnt support static functions, instead it supports the concepts of companion object. So fill() and tabulate() are part of companion object. You can see companion objects in scala-docs with (o) symbol on left and regular class as (c) as shown in the picture above.

**List**

----------------------------------
A list in Scala is immutable and is internally implemented as singly immutable linked list.


val mainList = List(3, 2, 1) //List[Int] = List(3, 2, 1)
val with4 = 4::mainList      //List[Int] = List(4, 3, 2, 1)
val with43 = 43::mainList   //with43  : List[Int] = List(43, 3, 2, 1)
mainList         //List[Int] = List(3, 2, 1)

//you can also build a list using cons

val lst1 = 1::2::3::Nil    //> lst1  : List[Int] = List(1, 2, 3)

Nil represents an empty list.

scala> val colors = List("red", "blue", "green")
//colors: List[java.lang.String] = List(red, blue, green)
scala> colors.head
//res0: java.lang.String = red
scala> colors.tail
//res1: List[java.lang.String] = List(blue, green)

//ToArray
val colors = List("red", "blue", "green")         //> colors  : List[String] = List(red, blue, green)
colors.toArray                                    //> res19: Array[String] = Array(red, blue, green)
colors.toVector                                   //> res20: Vector[String] = Vector(red, blue, green)

//updated (new list not mutable)

colors.updated(2,"yellow")                        //> res21: List[String] = List(red, blue, yellow)

//patch
colors.patch(2, List("orange","cyan"), 2)         //> res22: List[String] = List(red, blue, orange, cyan, magenta)

//diff
val colors2 = List("white","black","red", "blue") //> colors2  : List[String] = List(white, black, red, blue)

colors.diff(colors2)                              //> res25: List[String] = List(green, pink, magenta)

//mkstring
colors.mkString(",")                              //> res26: String = red,blue,green,pink,magenta
colors.mkString(" ")                              //> res27: String = red blue green pink magenta

//Zip
//zip
colors.zip(colors2)                               //> res28: List[(String, String)] = List((red,white), (blue,black), (green,red)
                                                  //| , (pink,blue))

colors.zipWithIndex                               //> res29: List[(String, Int)] = List((red,0), (blue,1), (green,2), (pink,3), (
                                                  //| magenta,4))

--------------------------------------

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

**Tuple**
Scala tuple combines a fixed number of items together so that they can be passed around as a whole. Unlike an array or list, a tuple can hold objects with different types but they are also immutable. Here is an example of a tuple holding an integer, a string, and the console

{% highlight scala %}
val t = (1, "hello", Console)
//or
val t = new Tuple3(1, "hello", Console)
{% endhighlight %}

The actual type of a tuple depends upon the number and of elements it contains and the types of those elements. Thus, the type of (99, "Luftballons") is Tuple2[Int, String]. The type of ('u', 'r', "the", 1, 4, "me") is Tuple6[Char, Char, String, Int, Int, String]
Tuples are of type Tuple1, Tuple2, Tuple3 and so on. There currently is an upper limit of 22 in the Scala if you need more, then you can use a collection, not a tuple. For each TupleN type, where 1 <= N <= 22, Scala defines a number of element-access methods. Given the following definition:

{% highlight scala %}
val t = (4,3,2,1)
val sum = t._1 + t._2 + t._3 + t._4
{% endhighlight %}

you can also create a tuple in this format, which comes handy when using maps

{% highlight scala %}
val tpl = 1 -> 2   //(1,2)
{% endhighlight %}

**Map**
Scala map is a collection of key/value pairs. Any value can be retrieved based on its key. Keys are unique in the Map, but values need not be unique. Maps are also called Hash tables. There are two kinds of Maps, the immutable and the mutable. The difference between mutable and immutable objects is that when an object is immutable, the object itself can't be changed.

By default, Scala uses the immutable Map. If you want to use the mutable Set, you'll have to import scala.collection.mutable.Map class explicitly. If you want to use both mutable and immutable Maps in the same, then you can continue to refer to the immutable Map as Map but you can refer to the mutable set as mutable.Map. Following is the example to declare immutable Maps as follows:

{% highlight scala %}
// A map with keys and values.
val colors = Map("red" -> "#FF0000", "azure" -> "#F0FFFF")

var A:Map[Char,Int] = Map()
A += ('I' -> 1)
A += ('J' -> 5)
A += ('K' -> 10)
A += ('L' -> 100)

{% endhighlight %}

{% highlight scala %}

object Test {
 def main(args: Array[String]) {
  val colors = Map("red" -> "#FF0000",
                   "azure" -> "#F0FFFF",
                   "peru" -> "#CD853F")

  val nums: Map[Int, Int] = Map()

  println( "Keys in colors : " + colors.keys )
  println( "Values in colors : " + colors.values )
  println( "Check if colors is empty : " + colors.isEmpty )
  println( "Check if nums is empty : " + nums.isEmpty )
 }
}

{% endhighlight scala %}

In Scala everything is objects, so when you specify 1+2 (internall its (1).+(2) , where '+' is a function in Int object and in a similar way as shown above for rangest 5 to 1 = (5).to(1). Its a rule in scala that if you have a function with 1 or 0 arguments you can use it with infix notation omitting the . and (). But its a best practice to put () for functions that has side effects/mutations