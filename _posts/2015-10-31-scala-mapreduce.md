---
layout: post
title:  "Scala : MapReducing"
date:   2015-10-31 8:50:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
---

In our previous <a target="_blank" href = "/scala-currying">post</a> we learned about currying using the concept of sumOf. In this post we will try to apply some Scala syntactical sugar on that function, write a productOf function , defining a factorial using that productOf function and then we will try to generalize sumOf and productOf, which can considered as a version of MapReducing.

**SumOF**

Let us start with sumOf in its basic higher order form.

{% highlight scala %}

def sumOf(f:Int => Int,a:Int, b:Int):Int = {

	if (a > b) 0
	else f(a) + sumOf(f,a+1,b)
	
}
sumOf(x=>x,1,5)

{% endhighlight %}

As a next step, let us modify it such that it returns a function.

{% highlight scala %}

def sumOf(f:Int => Int):(Int,Int) => Int = {

	def sums(a:Int, b:Int):Int = {
		if(a >b) 0
		else f(a) + sums(a+1, b)
	}
	sums
}
sumOf(x => x)(1,5)

{% endhighlight %}

Now apply currying syntax on the above implementation.

{% highlight scala %}

def sumOf(f:Int => Int)(a:Int,b:Int) = {

	 def sums(a:Int, b:Int):Int = {
   		if(a >b) 0
   		else f(a) + sums(a+1, b)
   }
   sums(a,b)
}
  
sumOf(x=>x)(1,5)


{% endhighlight %}

Now let us apply Scala syntactical sugar which allows us not to define the function definition and retun of function inside our function.

{% highlight scala %}

def sumOf(f:Int => Int)(a:Int, b:Int):Int = {
 	 if( a > b) 0
 	 else f(a) + sumOf(f)(a+1, b)
 }                                                
 sumOf(x=>x)(1, 5)      

{% endhighlight %}

**ProductOf**

Based on above Scala currying syntax we can write productOf as following.

{% highlight scala %}

def productOf(f:Int => Int)(a:Int, b:Int):Int = {
  	if (a >b) 1
  	else f(a) * productOf(f)(a+1,b)
  }                                               
  productOf(x=>x)(1,5) //res:120

{% endhighlight %}

So now we can write factorial based on productOf as following.

{% highlight scala %}

def factorial(n:Int) = productOf(x=>x)(1,n) 
factorial(5)       //> res2: Int = 120

{% endhighlight %}

**Generalizing**

Now if you look at sumOf and productOf, it looks like a candidate to generalize. Let us try that. 

{% highlight scala %}

def sumOf(f:Int => Int)(a:Int, b:Int):Int = {
 	 if( a > b) 0
 	 else f(a) + sumOf(f)(a+1, b)
 }                 

def productOf(f:Int => Int)(a:Int, b:Int):Int = {
  	if (a >b) 1
  	else f(a) * productOf(f)(a+1,b)
  }                  

{% endhighlight %}

So if you look closely into two functions, both accepts a mapping function that accepts an Int and returns an Int. The lower and upper bounds of parameters (a,b). Two things that differ in both functions are the unit value returned when bounds are crossed and the combining values + or *. Infact this can be considered as a version of mapreduce where the function (f) maps the values in the interval(a,b) and the reducer that combine the values to produce final results.

{% highlight scala %}

 def mapReduce(f:Int=>Int, combine:(Int,Int)=>Int, zero:Int)(a:Int,
 			   b:Int):Int =
  {
  	if(a >b) zero
  	else combine(f(a),mapReduce(f, combine, zero)(a+1, b))
  }
  //productOf in terms of mapReduce

  def productOf(f:Int => Int)(a:Int,b:Int) = 
  			mapReduce(f, ((x,y) => x*y), 1)(a, b)
  productOf(x=>x)(1,5) //> res3: Int = 120

  // sumOf in terms of mapReduce
  def sumOf(f:Int => Int) (a:Int, b:Int) = 
  		   mapReduce(f, ((x,y) => x+y), 0)(a,b)
  summOf(x=>x)(1,5)  //> res4: Int = 15


{% endhighlight %}












