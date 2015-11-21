---
layout: post
title:  "Scala : Basics"
date:   2015-10-24 7:40:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
---

Scala is an acronym for “Scalable Language”. This means that Scala grows with you.Up until a few years ago, to make applications perform better you installed a faster processor. But we’ve pretty much maxed out the speed limit for individual chips. So best way to get better performance now is to use chips with multiple cores, all operating at a high speed, and to spread the workload among them. So instead of writing a program that runs on a single core, you have to write a program that’s smarter about deploying the workloads among many cores.

Scala is started by Martin Odersky who is also the author of 'javac' compiler.“Single-core performance is running out of steam, and you need to parallelize everything,” Odersky told in a recent interview. You do that through what’s known as functional programming. Ray Ozzie, the former chief software architect for Microsoft and no slouch when it comes to coding, likens functional programming to spreadsheets where each cell in the spreadsheet containing that formula acts as an independent processor working concurrently to keep the spreadsheet updated. It’s a parallel computing system enabled by functional programming, Ozzie says via email.

Scala works with Java and compiles in the JVM, which is significant because many, many of the world’s enterprise applications are written in Java. In that way we can use our favorite Java libraries in scala too. Finally everything gets converted to Java byte code before running in JVM. 


**Command Line :**
When you type scala command in terminal, it can behave in 3 diffrent ways.
: $ scala (Just typing scala command will take us to REPL)
: $ scala hello.scala (will execute the script hello.scala)
: $ scalac hellow.scala (compiles to bytecode (.class), which can be executed using scala command)

**Types in Scala:**
In Java if you have noticed primitive types starts with lower case(int, double etc..) and other object types starts with capital case. In Scala by default there are no primitive types , every thing is of object type.In Scala Int, Float, Double etc.. are all object types . Internally Scala compiler will convert them to primitives. So from a Scala developer point of view, everything is an Object type and compiler will take care of rest. 
 
Given below shows Scala's type hierarchy.

![Scala Type Heirarchy](/img/scala-type-heirarichy.png)

At the top of heirarchy is the type "Any", that is everything in scala has a type "Any". From this type its divided into two types "AnyVal" and "AnyRef" . Things that we usually associates to primitive comes under "Anyval".  "AnyRef" is equivalent to Java's object type, so any other types in library or we create will come under this. 

A subtype of AnyRef can be Null, which is represented in the bottom of heirarchy. That means all types under AnyRef are nullable (but try using option type). Since Scala is functional in nature, every function should return something, so if you want a function to return something that is not valuable Unit is the return type. There is only one instance of unit represented by () and it won't print anything. Its equivalent to void in C/C++.  

There is also one more type under the diagram "Nothing" which is a subtype of absolutely every types in the scala. It is important to note that we cannot create a type of Nothing and only time you gets back a type of "Nothing" is during exception. It is important type considering the type inference. Let us say a function returns an int or if an exception occured it returns a nothing (which is also a subtype of int and absolutely every types in Scala). 

**Variables**
Now we have an understanding of types in scala, next step is to create variables of that types. Since scala does local type inference we might not need to specify the types in the variable declaration. You can make two types of declaration (val or var). A variable of type val is immutable(cannot be changed, similar to final in Java) and of type var is mutable. Immutable means value of the object referenced by that variable cannot be changed. (Scala prefers val type when possible). All variables in Scala should be initialized else it will result in compilation error. 

{% highlight scala %}

val x = 10  (val x:Int = 10)

var y = 20.9 (val y:Double = 20)

x = 30 // error

y = 33.2 

{% endhighlight %}

**Expressions vs Statments**
In other programming languages we use the term statements, but when it comes to Scala we should think in term of an expression.

A statement is one that does not return a value. For example:

{% highlight scala %}
class MyClass(val value: String)
{% endhighlight %}

Class definitions are statements, since they do not produce a value. If we tried to assign the result of this statement to a variable, we would make the compiler unhappy:

{% highlight scala %}
val incorrect = class MyClass(val value: String)
// Produces a compiler error with the messsage "illegal start of simple expression"
{% endhighlight %}

In contrast, expressions return a value that (hopefully) informs the caller of the result of the operation. Simple values such as 5 and "Hello" are statements, as well as many other constructs in Scala. Take the if-else structure as an example:
 
{% highlight scala %}

val x = 3
val result = if ( x > 2) "great" else "less"

{% endhighlight %}

The if/else expression takes the value of the branch selected at runtime. This value is captured and assigned to a variable. Furthermore, since the two branches produce values of type String, the compiler can assure result will be a String.

Additionally, we can group multiple expressions and statements to form a larger expression using curly braces:

{% highlight scala %}

val result =
  if(x % 2 == 0) {
   "Even"
  } else {
    "Odd"
  }                   

{% endhighlight %}

A multi-line expression will always take the value of its last subexpression. This principle is applied in a multitude of scenarios, but is particularly important since it allows us to leave out return instructions on method definitions:

Some constructs that are represented as statements in other languages are actually expressions in Scala. 

For Example :
: 1. if-else structures are statements in most languages, but are expressions in Scala. The type of an if-else expression is the common supertype of all the branches.
: 2. while loops are expressions in Scala that return Unit. Similarly, for loops (not to be confused with for expressions) also return Unit.
: 3. throw, used to produce exceptions, are expressions of type Nothing, which is a bottom type to all types in Scala.

**Match Expressions**
Match expressions are similar to switch statements in other languages but far more powerful. Let us see a simple example.

{% highlight scala %}

  val x = 10                                      //> x  : Int = 10
  
  val result = x match {
  case 1 => "One"
  case 2 => "Two"
  case _ => "I don't know"  
  }              
  // result  : String = I don't know
  
{% endhighlight %}
The above example is straight forward, its important to remember that in Scala, match expressions won't fall through, so no break statement is required and also '_' is taken as default case.

Now let us consider another case as shown below..

{% highlight scala %}
 val x = 10                                      //> x  : Int = 10
  
  val result = x*10 match {
  case 1 => "One"
  case 2 => "Two"
  case i => "I don't know " + i
  }
  
  //> result  : String = I don't know 100
  
{% endhighlight %}

In the above case, we can access the value by putting a lower case variable name (i). It doesn't have to be i any lower case variable name will work. 

You can also use tuples for pattern matching as shown below..

{% highlight scala %}

  val x = 10                                      //> x  : Int = 10
  val y = 20                                      //> y  : Int = 20
  
  val result = (x,y)match {
  case (1,_) => "One"
  case (_,20) => "Two"
  case i => "I don't know " 
  }            

  //> result  : String = Two


{% endhighlight %}

Let us see another case in which we need to check whether it matches another variable.

{% highlight scala %}

 val x = 10                                      //> x  : Int = 10
 val y = 20                                      //> y  : Int = 20
 val z = x + y                                   //> z  : Int = 30
  
 val result = x+y match {
 case 1 => "One"
 case `z` => "Summed"  // this will work only if z is val
 case i => "I don't know " + i
 }                                               //> result  : String = Summed

{% endhighlight %}
















