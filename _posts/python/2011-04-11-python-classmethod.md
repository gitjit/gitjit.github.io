---
layout: post
title:  "Python : @classmethod, @staticmethod and methods"
description: "Understanding @classmethod, @staticmethod and regular methods"
date:   2011-04-10
tags: [Python]
comments: true
references: [
   "Standard library : https://docs.python.org/3/library/index.html",
   "Dan Brader Channel : https://www.youtube.com/watch?v=PNpt7cFjGsM&t=681s&index=6&list=PLP8GkvaIxJP0VAXF3USi9U4JnpxUvQXHx",
]

excerpt: "In this post we will demystify the differences between @classmethod, @staticmethod and regular methods
in Python. We will discuss what is the diffrence and when to use it. After reading this post you will have a better
understanding on which method to use in your classes. "
---

In this post we will demystify the differences between @classmethod, @staticmethod and regular methods
in Python. We will discuss what is the diffrence and when to use it. After reading this post you will have a better
understanding on which method to use in your classes. 

Let us create a single class with following methods.  


```python
# oop.py

class MyClass:
    def method(self):
        return 'instance method called', self

    @classmethod
    def classmethod(cls):
        return 'class method called', cls
        
    @staticmethod    
    def staticmethod():
        return 'static method called'

```
Now first let us call the instance method. This method get the instance of the object
calling it as input param and it can read/edit the attributes of that object. So this method
can modify both object state and class state.

```
>>> myObj = oop.MyClass()
>>> myObj.method()
('instance method called', <oop.MyClass object at 0x00000228F0EBF710>)

```

Now let us call the classmethod, which gets the class object (In python everything is an object)
as input param and use that to call the class methods. This method cannot modify the object state,
but can modify the class state. We will look into a detail example below.

```python
>>> myObj.classmethod()
('class method called', <class 'oop.MyClass'>)

```
A static method is just a regular function, which has no connection to the actual class, but
making it static , scopes it to the class. You have to call it at class level not the object 
level. Its usually used to write utility functions associated with class.

```python
>>> >>> myObj.staticmethod()
'static method called'

```
You can also call classmethods and staticmethods in a class level. 

```python
>>> oop.MyClass.classmethod()
('class method called', <class 'oop.MyClass'>)
>>> oop.MyClass.staticmethod()
'static method called'

```  

In order to solidify our concept let us create a simple Pizza class. 

```python
# pizza.py

class Pizza(object):
    def __init__(self,ingredients):
        self.ingredients = ingredients

    def __repr__(self):
        return 'Pizza{}'.format(self.ingredients)

```

Now based on type of pizza we have to create it as shown below.  

```python
>>> margarita = Pizza(['cheese', 'tomattoes'])
>>> margarita
Pizza['cheese', 'tomattoes']
>>> prosciutto = Pizza(['cheese', 'tomattoes', 'ham', 'mushrooms'])
>>> prosciutto
Pizza['cheese', 'tomattoes', 'ham', 'mushrooms']

```
Consider this class has been actually using in a Pizza dealer software and everytime user adds an ingredient in
their website interally we have to create with ingredients, its a pain. This is where @classmethods become handy
to provide factory methods based on type of pizza. 

```python
# pizza.py
class Pizza(object):
    def __init__(self,ingredients):
        self.ingredients = ingredients

    def __repr__(self):
        return 'Pizza{}'.format(self.ingredients)

    @classmethod
    def margharita(cls):
        return cls(['cheese','tomattoes'])

    @classmethod
    def prosciutto(cls):
        return cls(['cheese','tomattoes', 'ham', 'mushrooms'])

```
Now we can use these factory methods for creating the required type of pizza as shown below.  

```python
>>> from pizza import Pizza
>>> Pizza.margharita()
Pizza['cheese', 'tomattoes']
>>> Pizza.prosciutto()
Pizza['cheese', 'tomattoes', 'ham', 'mushrooms'] 

```  
So now for understanding the @staticmethods let us add a method to calculate the area of the pizza. (May be to
classify as large, medium etc). I don't know a bad example..  So only goal here to show how to write and use
@static method in a class and ensure that they are written as a utility with in scope of a class rather than
a public function.

```python
# pizza.py
import math

class Pizza(object):

    pi = 3.14

    def __init__(self,ingredients, radius):
        self.ingredients = ingredients
        self.radius = radius

    def __repr__(self):
        return 'Pizza{}'.format(self.ingredients)

    def area(self):
        return self.calc_circle_area(self.radius)

    @classmethod
    def margharita(cls):
        return cls(['cheese','tomattoes'])

    @classmethod
    def prosciutto(cls):
        return cls(['cheese','tomattoes', 'ham', 'mushrooms'])

    @staticmethod
    def calc_circle_area(radius):
        return math.pi * (radius**2)

```

```python
>>> from pizza import Pizza
>>> margharita = Pizza.margharita()
>>> margharita.area()
314.1592653589793

```
So in a nutshell, use @staticmethods when you want to write a utility method with in scope of the class.
@classmethods are used often when we need to have multiple independent constructors. (factory methods).  

#### Using @classmethod as factory 

Another example of using @classmethod to provide a second constructor or factory method.  

Let us consider we have a custom date class "MyDate" as shown below.   

```python
class MyDate:
    def __init__(self, day, month, year):
        self.day = day
        self.month = month
        self.year = year

    def __str__(self):
        return '{}/{}/{}'.format(self.day,
                         self.month, self.year)
```
Now let us consider we need to create MyDate from a string representation. In this case we can add a 
new factory method using @classmethod as shown below.

```python
class MyDate:
    def __init__(self, day, month, year):
        self.day = day
        self.month = month
        self.year = year

    def __str__(self):
        return '{}/{}/{}'.format(self.day, self.month, self.year)

    @classmethod
    def create_date_from_string(cls, str_date):
        '''
         convert a string date (day-mon-year) to mydate
        :return:
        '''
        day, mon, year = str_date.split('-')
        return cls(day, mon, year)

```
Let us use it.

```
mydate = MyDate.create_date_from_string("12-12-79")
print(mydate)
# 12/12/79
```
_Coding is fun enjoy..._  


