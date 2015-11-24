---
layout: post
title:  "Scala : Higher Order Functions"
date:   2015-10-30 7:50:22
categories: Scala
author : Jithesh Chandrasekharan
image: sum_fn.png
comments: true
---

In functional programming, functions are first class citizens. Any function that can take other functions as arguments or can return functions as results are called as Higher Order Functions. All other functions are first-order functions. In mathematics higher-order functions are also known as operators or functionals. The derivative in calculus is a common example, since it maps a function to another function.First order functions operates on primitives like int, string etc. Let us consider the mathematical function sum_of and its implementations.

Let us consider implementing the mathematical expression "Sum Of" and its implementation in Scala to understand higher order functions.

As a first step let us consider a function that do Sum of Integers.

{% highlight scala %}

def sum_of_integers(a:Int, b:Int):Int =
{
	if (a > b)  0
	else
	{
	  a+sum_of_integers(a+1, b)
	}
}                                         
 
 sum_of_integers(1,3)

 //> res0: Int = 6                             

{% endhighlight %}

Now let us consider Sum of cubes.

{% highlight scala %}

def sum_of_cubes(a:Int, b:Int):Int =
 {
	if(a >b) 0
	else
	{
	  a*a*a + sum_of_cubes(a+1, b)
	}
 }                                                

sum_of_cubes(1, 3)  
//> sum_of_cubes: (a: Int, b: Int)Int   

{% endhighlight %}

Now let us consider Sum of factorials.

{% highlight scala %}

def sum_of_factorials(a:Int,b:Int) : Int=
{
   if(a > b) 0
   else
   {
   	  factorial(a)+sum_of_factorials(a+1, b)
   }
}                                         
	
sum_of_factorials(1, 3)  
//> res1: Int = 9                 
{% endhighlight %}

As you can see that we are seeing a pattern in which only the function that does some computation is changing and if we can make that function to be passed as an argument then we can rewrite the above function as following.

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
                                               	
sum_of_ints(1,3)    //> res2: Int = 6
sum_of_cubes(1,3)   //> res3: Int = 36
sum_of_facts(1, 3)  //> res4: Int = 9

{% endhighlight scala %}


So we can use a function type as an argument. Where “f” represents a function that accepts an Int as parameter and returns Int . Now we can call the sum_of() as higher order function.  Now we have seen that in the above function we are still repeating parameters a and b into sum function, Can it be even shorter getting rid of parameters ?  Answer is yes, and we will discuss that topic called "currying" in the next post. 

One of the most frequently used high order functions on scala collections are foreach, map and filter.

{% highlight scala %}

 val a = Array(1,2,3,4,5,6,7,8,9,10)      
 //> a  : Array[Int] = Array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
 a.foreach(println)           

 a.map(x => x*x)                               
  //> res5: Array[Int] = Array(1, 4, 9, 16, 25, 36, 49, 64, 81, 100)
   
 a.map(_*2)                                     
 //> res6: Array[Int] = Array(2, 4, 6, 8, 10, 12, 14, 16, 18, 20)
   
 a.filter(x => x%2 ==0)                         
 //> res7: Array[Int] = Array(2, 4, 6, 8, 10)

{% endhighlight %}








































