---
layout: post
title:  "Scala : Partial Functions"
date:   2015-11-05 8:50:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
---

In Scala a function can be either total or partial. A function is total if it is designed to work for any values of its domain. A canonical example for total function is "Multiplication",(a X b) whose domain is numbers and I can give any two numbers to the function and it produces a result. So it accepts any values in its domain.  Where as if you consider division (a / b), I can pass any number for a but b cannot be 0. So division is naturally a partial function.

A partial function is a function that does not provide an answer for every possible input value it can be given. It provides an answer only for a subset of possible data, and defines the data it can handle. In Scala, a partial function can also be queried to determine if it can handle a particular value.

**Total Function**

{% highlight scala %}

def multiply(a:Int, b:Int) = a*b

{% endhighlight %}

**Partial Function**
Given below is a partial function that accepts (Int,Int) as input argument and returns an Int. 
{% highlight scala %}

val safeDiv:PartialFunction[(Int,Int),Int] = {

   case(a,b) if b != 0 => a/b
}
safeDiv(10,2) //> res2: Int = 5 
safeDiv(10,0) //> res2: Int = 5 
safeDiv.isDefinedAt(1,0)  //> res3: Boolean = false

{% endhighlight %}

One advantage of partial function is that it supports 'isDefinedAt()' to ensure that input is supported by the function.In addition to this, you can combine multiple partial functions using "orElse".

{% highlight scala %}
val defDiv:PartialFunction[(Int,Int),Int] = {
		
	case(a,b) => a/b
}                                             

val comb = safeDiv orElse defDiv  //> comb  : PartialFunction[(Int, Int),Int]
{% endhighlight %}

**Partial Functions in disguise**
Partial functions are part of most of the collection methods. 
{% highlight scala %}
val map = Map(1 -> "Red", 2 -> "Green", 3 -> "Blue")
map(1)               //> res4: String = Red
map.isDefinedAt(4)   //> res5: Boolean = false
{% endhighlight %}





