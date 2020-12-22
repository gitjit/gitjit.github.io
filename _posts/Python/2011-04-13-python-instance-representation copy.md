---
layout: post
title:  "Python : Instance representation and Access control"
description: "Instant representation, attributes, naming conventions"
date:   2011-04-13
tags: [Python]
comments: true
references: [
   "Standard library : https://docs.python.org/3/library/index.html",
   "Python course : http://www.python-course.eu/python3_inheritance.php",

]

excerpt: "This post about Python magic methods.In python everything is an object and we are
applying some operations on this objects. But every operation have a  dedicated method behind the scene in
corresponding type implementation. These methods are called magic methods. "
---

In python when we create an instance , the attributes are internally stored in a python
dictionary. Dictionay plays a major part in internal representation of python objects. Let us consider 
a simple class `Person` as shown below.  

```python
class Person(object):
    def __init__(self, name, age, id ):
        self.name = name
        self.age = age
        self._id = id

    def get_lastname(self):
        return self.name.split(' ')[1]

```

Now let us access its attributes using the dictionary as shown below..

```python
>>> p = Person('Jithesh Chandrasekharan', 32,1)
>>> p.__dict__
{'name': 'Jithesh Chandrasekharan', '_id': 1, 'age': 32}
>>> p.__dict__['name']
'Jithesh Chandrasekharan'
>>> p.__dict__['age']
32

```
One major advantage(some times error prone) of this dictionary representation is that we can add attributes to an
object on the fly as shown below. In this case we are adding a new attribute on the instance.  

```python
>>> p.spam = 2
>>> p.__dict__
{'_id': 1, 'name': 'Jithesh Chandrasekharan', 'spam': 2, 'age': 32}
```

You can also delete instance variables as shown below..

`>>> del p.spam`

As you can see the instance `__dict__` holds only the attributes, so where are the methods ? Methods are part of 
class representation and hence we can access methods through `class.__dict__` as shown below. Each instance holds
a `__class__` variable as shown below and class holds a `__dict__` which has method entries.  


```python
>>> p.__class__
<class 'oop.Person'>
```
So when you call p.`get_lastname()`. Python is first resolving the class from `__class__` variable of the instance,
then it will get method from the internal `__dict__` of the class as shown below.

```python
>>> Person.__dict__
mappingproxy({'__dict__': <attribute '__dict__' of 'Person' objects>, '__init__':
<function Person.__init__ at 0x0000014087BD5488>, 
'__weakref__': <attribute '__weakref__' of 'Person' objects>,
'__module__': 'oop', '__doc__': None,
'get_lastname': <function Person.get_lastname at 0x0000014087BD5510>})

```
Now it will get the method from the `__dict__` and calls the method passing instance as parameter.

```python
>>> Person.__dict__['get_lastname']
<function Person.get_lastname at 0x0000014087BD5510>
>>> Person.__dict__['get_lastname'](p)
'Chandrasekharan'

```

#### Access Modifiers ?  
One of the biggest advantage here is flexibility, but it raises an important question when writing a
large program, how we can control access to these attributes. There is no notion of private/public
in python. It largely depends on naming conventions. So  variables prefixed with underscore `_id`  are
considered as private. This naming convention is applicable to methods too. But naming convention really
solves the problem ? Its not actually restricting any access. So let us see how we can implement some control
around attributes. 

As a first step let us see how we can take ownership of an attribute using properties in Python.

Let us start with a simple Holding class as shown below.  

```python
class Holding(object):
    def __init__(self, name, date, shares, price):
        self.name = name
        self.date = date
        self.shares = shares
        self.price = price
        
    def cost(self):
        return self.shares*self.price
```  
In this example, we want to ensure that `shares` should be an `int` and `price` should be `float`. In order
to enforce these restriction we can make these two attributes as property (`@property`) as shown below.  

```python
class Holding(object):
    def __init__(self, name, date, shares, price):
        self.name = name
        self.date = date
        self.shares = shares
        self.price = price

    @property
    def shares(self):
        return self._shares

    @shares.setter
    def shares(self,new_shares):
        if not isinstance(new_shares,int):
            raise TypeError('Expecting an int !')
        self._shares = new_shares

    @property
    def price(self):
        return self._price

    @price.setter
    def price(self,new_price):
        if not isinstance(new_price, float):
            raise TypeError('Expecting a float')
        self._price = new_price
    
    @property
    def cost(self):
        return self.shares*self.price

```
So what we have done here is hiding the actual `price` and `shares` in private variable `_price` and `_shares`. 
Then we made our `price` and `shares` as properties. So now the internal representation is as shown below. So
we encapsulated our shares and price attributes.

```

hld1 = Holding('JJJ','23-2-2222',22,33.00)
{'name': 'JJJ', '_shares': 22, 'date': '23-2-2222', '_price': 33.0}

```
This way we can have a type checking before setting the value. In this case we ensure that correct types are passed.  

```python
 @property
    def shares(self):
        return self._shares

    @shares.setter
    def shares(self,new_shares):
        if not isinstance(new_shares,int):
            raise TypeError('Expecting an int !')
        self._shares = new_shares

```
Now this will raise an exception.Here we are passing a string for shares instead of int.

```python
hld1 = Holding('JJJ','23-2-2222','2232',33)

raise TypeError('Expecting an int !')
```
Now another important change we made here is to change our cost method to a property. This way
user can access `cost` method like a property.

```python
   @property
    def cost(self):
        return self.shares*self.price
```

_Coding is fun enjoy..._  


