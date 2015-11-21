---
layout: post
title:  "Scala : Recursion"
date:   2015-10-24 7:40:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
---

Recursion means "defining a problem in terms of itself". This can be a very powerful tool in writing algorithms. Recursion comes directly from Mathematics, where there are many examples of expressions written in terms of themselves. For example, the Fibonacci sequence is defined as: F(i) = F(i-1) + F(i-2). Recursion is the process of defining a problem (or the solution to a problem) in terms of (a simpler version of) itself. 

For example, we can define the operation "find your way home" as:
: 1.If you are at home, stop moving.
: 2.Take one step toward home.
: 3."find your way home"

Before digging deep into recursion let us first understand a basic function syntax in scala.
**Functions**
In Scala functions are first class citizens and we define a function as follows.

{% highlight scala %}

def square(x:Int):Int = x*x                     //> square: (x: Int)Int  
square(10)
                                      //> res4: Int = 100
{% endhighlight %}

In a function, we must specify type of input parameters and you can ignore return type and leave it to Scala type inference, but its a best practice to specify the return type before "=". "=" is adapted from functional programming and its what denotes the compiler where the function body starts and improves the readability. In this example, we have defined in a single line since its only a single expression. But in larger functions we can use code block, where the last subexpression will be returned. Its important to note that, scala supports return keyword and if we are specifying a return statement, ensure that we are specifying return type in function declaration.

{% highlight scala %}

 def isEven(x:Int):Boolean =
  {
  	if (x%2 == 0) true
  	else false
  }                                               //> isEven: (x: Int)Boolean
  
  isEven(10)                                      //> res5: Boolean = true

{% endhighlight %}

Now let us consider recursive function and easiest sample to start with is "Factorials". Its important to not that recursive functions should have return type specified.

**Factorial**

n! = 1x2x3x...(n-1)x(n)

{% highlight scala %}

def factorial(n:Int):Int = {  
  if (n <=1) 1
  else n * factorial(n-1)  
  }                                               //> factorial: (n: Int)Int
  factorial(5)                                    //> res6: Int = 120

{% endhighlight %}

There is a draw back in this implementation. Its that call stack is increasing with each recursive call. The call stack looks like this. 

{% highlight bash %}
  return 1
  ---------------
  1 * facotrial(0)
  ----------------
  2 * factorial(1)
  --------------
  3 * factorial(2)
  ---------------
  4 * factorial(3)
  -----------
  5 * factorial(4)
  ---------------
{% endhighlight %}

That is each computation is pushed the call stack until we reached the end. So if you push a bigger value the stack will over flow and cause an exception. we will discuss how to solve this in our next post on "tail recursion"

**Estimate value of Pi**

In this example we will be trying to estimate value of pi using a unit circle as shown in the picture. 

![Estimate PI ](/img/ucircle.png)

It represents a unit circle embedded inside a unit square. 

{% highlight bash %}
Area of square = 1 (length * breadth) 
Area of circle PI x r x r , where r = 1/2 = PI/4.
{% endhighlight %}

So logic here in estimating pi using this method is as follows. If we start throwing darts into this square, then 

{% highlight bash %}

Number of darts hit inside the circle        Area of Circle       PI
-------------------------------------   ~ =  --------------  =    ---
Total darts hit the board                    Area of Square       4  

       Number of darts hit inside the circle
 PI =  -------------------------------------  X 4
       Total darts hit that board

{% endhighlight %}

So this problem is a real candidate for recursion and the accuracy of this increases with number of darts thrown.Let us try to implement this problem.

{% highlight scala %}
 def calculatePI(n:Int):Double = {
    // throw dart and count hits inside circle
    def throwDartAndCount(n:Int):Double =   
    {
      if (n < 1) 0
      else
      {
        val x = math.random
        val y = math.random
        // ensure point is with in circle
        (if(x*x + y*y < 1)1 else 0) + throwDartAndCount(n-1)
      }
    }
    throwDartAndCount(n)/n*4
  }                                 //> calculatePI: (n: Int)Double
  calculatePI(1000)                //> res7: Double = 3.176
  
{% endhighlight %}

So after throwing 1000 darts we got a value ~3.17. We can improve the result by increasing the number of darts. Let us try 10,000 darts and see the result. 

{% highlight scala %}

 estimatePI(10000)             //> java.lang.StackOverflowError
                           //| 	at java.util.Random.nextDouble(Random.java:532)

{% endhighlight %}

So as we increased the iterations, we got a stack flow error !  How can we solve this ? The solution to this problem in Tail Recursion, which we will discuss in next post.



