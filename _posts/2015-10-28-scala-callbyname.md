---
layout: post
title:  "Scala : Call By Name"
date:   2015-10-28 7:50:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
---

In Scala, we can pass parameters to a function in two diffrent ways. Call by Value and Call by Name. By default it is  Call By Value, where the value of parameter is evaluated before passed to function. Call By Name is used, when we write function that accept a parameter as an expression and we want it to get evaluated until it's called with in the function. Let us try it with an example.

**Call By Value**
{% highlight scala %}

 def time() = {
      println("Getting time in nano seconds")
      System.nanoTime
   }
   def delayed( t: Long ) = {
      println("In delayed method")
      println("Param: " + t)
      t
   }

 // output
  Getting time in nano seconds
  In delayed method
  Param:30038920239964
  res0:Long = 30038920239964

{% endhighlight %}

So in this case, you can see the both calls in delayed() is outputting same time in nano seconds. Now let us try Call By Name.A call-by-name mechanism passes a code block to the callee and each time the callee accesses the parameter, the code block is executed and the value is calculated. Here, we declared the delayed method, which takes a call-by-name parameter by putting the => symbol between the variable name and the type.

{% highlight scala %}


object test {

 def get_time() = {
      println("Getting time in nano seconds")
      System.nanoTime
   }                                              //> get_time: ()Long
   def delayed( t: => Long ) = {   //call by name
      println("In delayed method")
      println("Param: " + t)
      t
   }                                            
}

//output

  //> In delayed method
  //| Getting time in nano seconds
  //| Param: 30347585241671
  //| Getting time in nano seconds
 //| res0: Long = 30347585440995
                               					

{% endhighlight %}

Here, delayed prints a message demonstrating that the method has been entered. Next, delayed prints a message with it's value. Finally, delayed returns t.




