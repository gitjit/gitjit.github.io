---
layout: post
title:  "Scala : Currying"
date:   2015-10-31 7:50:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
---

In mathematics and computer science, currying is the technique of translating the evaluation of a function that takes multiple arguments (or a tuple of arguments) into evaluating a sequence of functions, each with a single argument. It was introduced by Moses Schönfinkel and later developed by Haskell Curry.

We have encountered this in our earlier post in collection methods (fill and tabulate)

{% highlight scala %}
val arr4 = Array.fill(10)(0)  
//> arr4  : Array[Int] = Array(0, 0, 0, 0, 0, 0, 0, 0, 0, 0)

val arr1 = Array.tabulate(10)(i => i*i) /
// index based filling.

{% endhighlight %}

In our previous <a target="_blank" href = "/higherorder">post</a>, we discussed higher order functions in scala and we found that in sum_of function we were taking multiple arguments and let us use currying technique to optimize that. 
{% highlight scala %}

def sum_of(f:Int=>Int, a:Int, b:Int):Int=
{
	if (a > b) 0
	else
	{
		f(a) + sum_of(f,a+1,b)
	}
}                                         	

{% endhighlight %}

Now we can redefine our functions as following..

{% highlight scala %}

def sum_of_ints(a: Int,b:Int) = sum_of(x=>x,a,b)
                                                  
def sum_of_cubes(a:Int,b:Int) = sum_of(x=>x*x*x, a, b)
                                                 
def sum_of_facts(a:Int,b:Int) = sum_of(factorial,a,b)
                                               	

{% endhighlight scala %}

So we can use a function type as an argument. Where “f” represents a function that accepts an Int as parameter and returns Int . Now we can call the sum_of() as higher order function.  Now we have seen that in the above function we are still repeating parameters a and b into sum function, Can it be even shorter getting rid of parameters ? We can use currying technique here.

{% highlight scala %}

def sum_of(f:Int => Int):(Int,Int) => Int =
{
	def sum_of(x:Int, y:Int) :Int =
	{
		 if (x > y) 0
		 else
		 {
		 	 f(x) + sum_of(x+1, y)
		 }
	}
	sum_of
}     
//> sum_of: (f: Int => Int)(Int, Int) => Int
	
def sum_of_ints = sum_of(x=>x)            
def sum_of_cubes = sum_of(x=>x*x*x)       
def sum_of_facts = sum_of(factorial)  

sum_of_ints(1,3)     //> res1: Int = 6
sum_of_cubes(1,3)    //> res2: Int = 36
sum_of_facts(1,3)    //> res3: Int = 9
	

{% endhighlight scala %}

So what we have done here is "sum_of" function returns a function that returns another function that takes two arguments and returns an int.  

In the previous example can we avoid the middle men sum_of_ints, sum_of_cubes and sum_of_facts ?  Of Course if we rewrite it as following.

{% highlight scala %}

sum_of(x=>x)(1,3)     //> res4: Int = 6
sum_of(x=>x*x*x)(1,3) //> res5: Int = 36
sum_of(factorial)(1,3) //> res6: Int = 9

{% endhighlight %}

So in the above example, sum_of(x=>x) is equivalent to sum_of_ints , similarly other functions equivalent to sum_of_cubes and sum_of_facts respectively. Then the function is next applied to arguments (1,3). Generally function application associates to left:

sum_of(factorial)(1,3) = (sum_of(factorial))(1,3). So at this point certainly a question arises what is the point of doing this way rather than passing 3 parameters. The advantage here is that sum_of(factorial) it self is a valid expression with out passing other parameters and can be applied later based on some parameters we obtained during some calculations.

This technique of translating the evaluation of a function that takes multiple arguments into sequence of functions that takes single argument is called Currying.












































