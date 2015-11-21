---
layout: post
title:  "Scala : Tail Recursion"
date:   2015-10-26 7:40:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
---

In our previous post we discussed the recursion and  as we increased the number of iterations we got "Stack Over Flow " error. So solution to that is doing a "Tail Recursion".Recursive procedures call themselves to work towards a solution to a problem. In simple implementations this balloons the stack as the nesting gets deeper and deeper, reaches the solution, then returns through all of the stack frames. This waste is a common complaint about recursive programming in general.

A function is called as tail recursive function, if the last thing it has to do is to call itself.A compiler is said to implement TailRecursion if it recognizes this case and replaces the caller in place with the callee, so that instead of nesting the stack deeper, the current stack frame is reused. This is equivalent in effect to a "GoTo", and lets a programmer write recursive definitions without worrying about space inefficiency (from this cause) during execution. TailRecursion is then as efficient as iteration normally is.
That is not the case in our factorial nor calculatePI examples. So we will convert our functions to tail recursive. In these examples I am using a accumulator which holds the results in each iterations.

**Factorial - Tailed**

{% highlight scala %}
def factorial(n: Int): Int =
    {
      def aux(n: Int, acc: Int): Int =
        {
          if (n <= 1) acc
          else {

            aux(n - 1, acc * n)
          }
        }
      aux(n, 1)
    } 

  factorial(5) //> res0: Int = 120

{% endhighlight %}

The advantage here is that no new operations are pushed to stack and hence avoid stack growth irrespective of the number of iterations. One thing Scala does internally when it detects a tail recusive code is, it converts it in byte code to a while loop.

**PICalculator - Tailed**

{% highlight scala %}
import scala.annotation._
  def CalculatePI(n: Int): Double =
    {
      @tailrec
      def helper(n: Int, acc: Double): Double =
        {
        
        	if(n <=1) acc
        	else {
        		val x:Double = math.random
        		val y:Double = math.random
        		helper(n-1, acc + (if(x*x + y*y < 1) 1 else 0))
        		
        	}

        }
      helper(n, 0)/n*4
    }                         //> CalculatePI: (n: Int)Double
    
    CalculatePI(10000)       //> res1: Double = 3.14

{% endhighlight %}

As you can see we have increased our iterations to 10,000 and obtained an accurate result with out a stack over flow error. Also note that I have added an annotation @tailrec, so that Scala will complain if that implementation cannot be optimized for tail recursion.



