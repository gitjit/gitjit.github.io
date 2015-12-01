---
layout: post
title:  "Scala : Classes and Constructors"
date:   2015-10-24 7:40:22
categories: Scala
author : Jithesh Chandrasekharan
image: 
comments: true
meta: Scala Classes.
---

Classes in Scala are static templates that can be instantiated into many objects at runtime.Once you define a class, you can create objects from the class blueprint with the keyword new. Following is a simple syntax to define a class in Scala:

{% highlight scala %}
class Greet { 
  println("Hello OOPs") 
  }
 val gr = new Greet()  //> Hello OOPs
{% endhighlight %}

Once we instantiated the class, the println gets called. The reason is that by defaul any code inside the code block is part of default constructer of the class and gets executed when instantiated. Now let us create another class for Cars.

{% highlight scala %}
class Car(val name:String, var model:String){
  	println("Inside Car Constructor")
  	private var cur_speed = 0 // field
  	def start() = {
  	
  		println("Started... Rrrr Rrrr")
  		cur_speed = 0
  		
  	}
  	def accelerate(x:Int) = {
  		if(x < 5) cur_speed +=5
  		else if(x < 10){ cur_speed += 10 }
  		else { cur_speed += 20}
    		
  	} 	
  	def getSpeed() = cur_speed 	
  	override def toString() =
  	{
  			"Car is " + name +
  			" and model is " + model
  	}
  	
  }
  
  val mazda = new Car("mazda", "protege") //Inside Car Constructor
{% endhighlight %}
Now we have created a class here with three fields name, model and cur_speed.The constructor arguments get converted into read only fields for val and read/write fields for vars. (If you don't specify val/var then those are by default private parameters).We have also defined three methods and a method to override toString().
{% highlight scala %}
 val mazda = new Car("mazda", "protege")
  mazda.name     //> res2: String = mazda
  mazda.model   //> res3: String = protege
  
  mazda.model = "mazda3"
  mazda.model      //> res4: String = mazda3
  val str = mazda.start() //> Started... Rrrr Rrrr
                                                 
  mazda.accelerate(10) //| str  : Unit = ()
  mazda.getSpeed()    //> res5: Int = 20
  mazda //> res6: samples.Car = Car is mazda and model is mazda3
{% endhighlight %}

We are able to edit model here because its of var type. 

**Multiple Constructors**
The Scala approach to defining multiple class constructors is a little different than Java, but somewhat similar. Let us try this by extending our previous example.
{% highlight scala %}
class Car(val name:String, val model:String, val mileage:Int){
  	println("Inside Car Constructor")
  	private var cur_speed = 0  	
  	def this(name:String) =
  	{
  		this(name,"Protege", 20)
  	}	
  	def this(name:String, model:String) =
  	{
  		this(name,model, 20)
  	} 	
  	def this(mileage:Int) = {
  		this("Mazda", "Protege", mileage)
  	}
  	override def toString() =
  	{
  			"Car is " + name +
  			" ,model is " + model +
  			" and mileage is " + mileage
  	}
  	
  }
{% endhighlight %}

Now let us instantiate with diffrent constructers.
{% highlight scala %}

val mazda = new Car("mazda") //> Inside Car Constructor                            
mazda //> res2: samples.Car = Car is mazda ,model is Protege and mileage is 20
  
val benz = new Car("benz","s-class") //> Inside Car Constructor
benz  //Car is benz ,model is s-class and mileage is 20
  
val mazda3 = new Car(15)//> Inside Car Constructor
mazda3 //Car is Mazda ,model is Protege and mileage is 15

{% endhighlight %}
**Extending Classes**
Scala does not allow multiple inheritance of classes, but you can use traits for that. Now let us consider a base class called Auto with new fields category and seats. It also has a method hasNitro. Let us see how to inherit from this base class and override hasNitro in the subclass. Its important that the subclass constructor should invoke base class constructor as shown below.

{% highlight scala %}
class Auto(val category:String){
  
	val seats:Int = if (category == "sedan") 4 else 8	
	def hasNitro() = true
  	
  }
  
 class Car(val name:String,val model:String, val mileage:Int,cat:String) 
  		extends Auto(cat)
  {
  	println("Inside Car Constructor")
  	private var cur_speed = 0
  	def this(name:String) =
  	{
  		this(name,"Protege", 20,"sedan")
  	}	
  	def this(name:String, model:String) =
  	{
  		this(name,model, 20,"suv")
  	}	
  	def this(mileage:Int) = {
  		this("Mazda", "Protege", mileage, "sedan")
  	}
  	override def toString() =
  	{
  			"Car is " + name +
  			" ,model is " + model +
  			" and mileage is " + mileage
  	}
  	override def hasNitro() = false 	
  }
{% endhighlight %}

Now let us instantiate..

{% highlight scala %}
 val mazda3 = new Car(15) //> Inside Car Constructor
  mazda3.category  //> res2: String = sedan
  mazda3.seats //> res3: Int = 4
  mazda3.hasNitro() //false  
{% endhighlight scala %}




