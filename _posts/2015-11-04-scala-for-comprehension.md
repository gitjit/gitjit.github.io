---
layout: post
title:  "Scala : For Comprehensions"
date:   2015-11-04 8:50:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
meta: For loop in Scala is not a simple loop, but its actually a For Comprehension.
---

For loop in Scala is not a simple loop, but its actually a For Comprehension.A For Comprehension not only offers the ability to iterate over something like a List, it also provides filtering options and the ability to generate new Lists. In this example, I am just brushing up the basic for comprehension syntax and we will look into much elaborate sample later.

Let us first try a while loop.

{% highlight scala %}
var i = 0                                 //> i  : Int = 0
while(i < 5){

	println(i)
	i+=1
}            
{% endhighlight %}

Here the while loop is just a statement not an expression (which returns a value). Now let us switch our focus to for loop.

{% highlight scala %}
for (i <- 0 to 5) println(i) // 1,2,3,4,5
{% endhighlight %}

In the for loop '<-' is part of Scala syntax and is read as 'in'. It can be specified with any collections. 

{% highlight scala %}
val arr = Array.fill(5)(Math.random()) 
for (i <- arr) println(i)
{% endhighlight %}

In the above statement, for loop is working as a statement as it returns nothing. we can make for loop and expression using 'yield' statement. 

{% highlight scala %}
val arr = Array.fill(5)(Math.random()) 
for (i <- arr) yield i*i
// similar to
arr.map(i => i*i)
{% endhighlight %}

for loop in Scala are actually 'For comprehensions'. Let us see how we can put multiple generators, conditions and variable declaration inside the for loop.

{% highlight scala %}
for ( i <- 1 to 5; if(i%2 == 0); a = 2*i; j <- 6 to 10) yield(a,j)
//  Vector((4,6), (4,7) ...
                                                
{% endhighlight %}

we can rewrite the above syntax as following.
{% highlight scala %}
for ( i <- 1 to 5;
      if(i%2 == 0);
      a = 2*i;
      j <- 6 to 10) yield(a,j)   
{% endhighlight %}

This will work since we seperate with semicolon.  But we can avoid semicolon, if we put it inside a code block {} as shown below.

{% highlight scala %}
 for { i <- 1 to 5
      if(i%2 == 0)
      a = 2*i
      j <- 6 to 10}
       yield(a,j) 
{% endhighlight scala %}

As you can see interally the above for comprehension will get converted to map, filter and flatmap calls on collections.

