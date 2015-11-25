---
layout: post
title:  "Scala : Pattern Matching"
date:   2015-11-02 8:50:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
---

Pattern matching is one of the killer feature in Scala, which is hard to find in some other programming languages.Pattern matching tests whether a given value (or sequence of values) has the shape defined by a pattern, and, if it does, binds the variables in the pattern to the corresponding components of the value (or sequence of values). The same variable name may not be bound more than once in a pattern.

For example:

1. The pattern Some(x) matches values of the form Some(v), binding x to the argument value v of the Some constructor.
2. The pattern (x, _) matches pairs of values, binding x to the first component of the pair. The second component is matched with a 
   wildcard pattern.
3. The pattern x :: y :: xs matches lists of length ≥2, binding x to the list's first element, y to the list's second element,and xs to the remainder

First let us try some basic pattern matching sytax.

{% highlight scala %}
 val x = 50   
 val y = x match
 {
	case 50 => "fifty"
	case _  => "no match" //default
 			
 }                                                
 y  //> y  : String = fifty

{% endhighlight %}

{% highlight scala %}
val Array(h,m,s) = "5:4:3".split(":") 
h // 5
m // 4
s // 3
{% endhighlight %}

{% highlight scala %}

val lst = List(1,2,3,4,5)                         //> lst  : List[Int] = List(1, 2, 3, 4, 5)
val ret = lst match
{
  case x::y::z => y
}                                                
ret  /> res1: Int = 2

{% endhighlight scala %}

**Length of List using pattern matching**
we can build a list using cons. so we can use cons to do a pattern matching too.
   
{% highlight scala %}
val lst = 1::2::3::4::5::Nil //List[Int] = List(1, 2, 3,4,5)
def listLength(lst:List[Int]):Int = lst match
 {
	case Nil => 0
	case h::t => 1 + listLength(t)
 }                                                //> listLength: (lst: List[Int])Int
 listLength(lst)  //5
{% endhighlight scala %}

**Option Type**
{% highlight scala %}
lst.find(_>3)match
{
	case None => "Not Found"
	case Some(x) => "Found " + x
}                            
// res3: String = Found 4                     
                     
{% endhighlight %}


