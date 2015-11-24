---
layout: post
title:  "Scala:Built-in High Order Methods"
date:   2015-11-01 8:50:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
---

Now we have a good understanding of high order functions and how they works. Now let us look into some built-in high order methods in scala collections and see how handy they are.

**foreach**
Applies a function f to all elements of this array.

*def foreach(f: (A) ⇒ Unit): Unit* 

foreach construct can be considered as more imperative and not functional, since it won't return any values. As shown below foreach basically takes each items in it collection and pass it as a parameter to the function specified.

{% highlight scala %}

val arr = Array(1,2,3,4,5)
arr.foreach(println)
// equivalent to shown below
arr.foreach(x => println(x))

// 1 2 3 4 5

{% endhighlight %}

**map**
Builds a new collection by applying a function to all elements of this array. So map takes an input collection, it transforms each element in the input collection and returns a new collection, with each element in the input transformed. The type of output collection depends on the transformation function. 

*def map`[B]`(f: (A) ⇒ B): Array[B]* 

B : the element type of the returned collection.
f : the function to apply to each element.
returns : a new array resulting from applying the given function f to each element of this array and collecting the results.
The type of resultant array depends on the map operation.

{% highlight scala %}
val arr = Array(1,2,3,4,5)
//map
arr.map(i => i*2)                                 
//> res0: Array[Int] = Array(2, 4, 6, 8, 10)
//or if every arg occurs once and in order. Here 
// we are replacing 'i => i' with '_'
arr.map(_*2)                                      
//> res1: Array[Int] = Array(2, 4, 6, 8, 10)
//converts to a collection of new types
arr.map(i => i +" is number")                     
//> res2: Array[String] = Array(1 is number, 
  2 is number, 3 is number, 4 is number, 5 is number)
or
arr.map(_+" is number")


{% endhighlight %}

**filter**

Selects all elements of this mutable indexed sequence which satisfy a predicate.

def filter(p: (T) ⇒ Boolean): Array[T]

p : the predicate function that returns a Boolean

{% highlight scala %}

val lst = List(6,7,8,9,10)
lst.filter(i => (i%2 == 0))  // List(6, 8, 10)
// or 
lst.filter(_%2 ==0)
lst.filter(_>5)  // List(6, 7, 8, 9, 10)

{% endhighlight %}

**flatten**
Flattens a two-dimensional array by concatenating all its rows into a single array.

{% highlight scala %}

val arr = Array(1,2,3,4,5)
val take_index = arr.map(i => arr.take(i)) 
// take_index  : Array[Array[Int]] = Array(Array(1), Array(1, 2), 
//Array(1, 2, 3),Array(1, 2, 3, 4), Array(1, 2, 3, 4, 5))

val tk_flat = take_index.flatten 
//Array[Int] = Array(1, 1, 2, 1, 2, 3, 1, 2, 3, 4, 1, 2, 3, 4, 5)

tk_flat.distinct
Array[Int] = Array(1, 2, 3, 4, 5)

{% endhighlight %}

**flatMap**
Builds a new collection by applying a function to all elements of this array and using the elements of the resulting collections. It can be considered as applying map and then flatten. 

{% highlight scala %}

val arr = Array(1,2,3,4,5)
val take_index_flat = arr.flatMap(i => arr.take(i))
//Array[Int] = Array(1, 1, 2, 1, 2, 3, 1, 2, 3, 4, 1, 2, 3,
                                                  //|  4, 5)
{% endhighlight %}

**exists and forall**
{% highlight scala %}
val lst = List(6,7,8,9,10)
lst.exists  (_>10)  //> res7: Boolean = false
lst.forall(_<11)   //> res8: Boolean = true
{% endhighlight %}

**reduceLeft**
Applies a binary operator to all elements of this mutable indexed sequence, going left to right. In this example, it calculates x+y and then adds 3 with x+y and so on..
{% highlight scala %}

val arr = Array(1,2,3,4,5)
arr.reduceLeft((x,y) => (x+y))                    
//> res9: Int = 15 
// it can be also written as following
arr.reduceLeft(_ + _)    


{% endhighlight %}

**foldLeft**
Applies a binary operator to a start value and all elements of this mutable indexed sequence, going left to right.
{% highlight scala %}

// it starts with x as 1, so 1+1, 2+2 etc..
arr.foldLeft(1)(_+ _)   //> res10: Int = 16
// starts with x as "0" and hence "01" + "2"
arr.foldLeft("0")(_+_)  //> res11: String = 012345

{% endhighlight %}

                                         


