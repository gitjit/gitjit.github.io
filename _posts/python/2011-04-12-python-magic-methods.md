---
layout: post
title:  "Python : Magic Methods"
description: "Magic methods in Python"
date:   2011-04-12
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

In python everything is an object and we are applying some operations on this objects. But every operation have a 
dedicated method behind the scene in corresponding type implementation.  

```bash
>> x = 10
>> x + 10 is equivalent to x.__add__(10)
>> x * 2 is equivalent to x.__mul__(2) 
>>> names = ['A', 'AB', 'ABC']
>>> names[0] is equivalent to names.__getitem__(0)
>>> names.__setitem__(0, 'BAB')
>>> names is now ['BAB', 'AB', 'ABC']

```
Now let us use this in a custom class.  

```python
class Point:
    def __init__(self,x ,y):
        self.x = x
        self.y = y

    def __add__(self,other):
        return Point(self.x + other.x,self.y + other.y)

    def __str__(self):
        return 'Point x= {} and y = {}'.format(self.x, self.y)

p1 = Point(10,20)
p2 = Point(1,2)
p3 = p1 + p2

print(p3)

```

outputs :  
`Point x= 11 and y = 22`

####  str vs repr

While debugging if you evaluate our point object , it will be displayed like this.  

```
>>p1
>><__main__.Point at 0x24961fbf898>`


```
`__repr__` is used during debugging a lot, its called representation of the object and used widely for debug only purpose.
'__str__' is used for mainly for printing info.

```  

>>d1 = datetime.date(2012, 12,24)
>>d1
outputs:
datetime.date(2012, 12, 24)
>>print(d1)
outputs:
>>2012-12-24

```
So now let us add this support to our point class as shown below

```python
class Point:
    def __init__(self,x ,y):
        self.x = x
        self.y = y

    def __add__(self,other):
        return Point(self.x + other.x,self.y + other.y)

    def __str__(self):
        return 'Point x= {} and y = {}'.format(self.x, self.y)
    
    def __repr__(self):
        return('Point({!r},{!r})'.format(self.x, self.y))

>>p1 = Point(10,20)
>>p1
outputs : Point(10,20)
>>print(p1)
outputs: Point x= 10 and y = 20

```
So when writing your own class ensure to include `__str__` and `__repr__` as best practice.  

#### Context Managers  

The basic resource management pattern we have been following so far is `open resource, use it and close it`.
In case of file, we have been doing this safeguarding using `with` statement.   

` with open(filename, 'r') as f:` 

Internally this is implemented using two magic methods `__enter__ and __exit__`. So let us consider 
our point class is a resource and needs to do some clean up after its usage. Then in that case we 
can add context managers to our class as follows..  

<pre class='line-numbers'>
<code class='language-python'>
class Point(object):
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)

    def __str__(self):
        return 'Point x= {} and y = {}'.format(self.x, self.y)

    def __repr__(self):
        return ('Point({!r},{!r})'.format(self.x, self.y))

    def __enter__(self):
        print('Entering')
        return "A sample P"

    def __exit__(self, ty, val, tb):
        print('Exiting')
        print(ty, val, tb)
</code>
</pre>  

As shown above (line:17)the return value of entering is enabling us to use `as` statement along `with`
Now we can use this in `with` context as shown below..  

```python
p = Point(10,20)
with p as val:
    print(p)
    print(val)

```    

outputs: 

```
Entering
Point x= 10 and y = 20
A sample P
Exiting
None None None

```
The paramters in `__exit__` can be used to capture the exceptions and deal based on that.  

```
p = Point(10,20)
with p as val:
    print(p)
    print(9/0)
    print(val)

```
outputs:
```
Entering
Traceback (most recent call last):
Point x= 10 and y = 20
  File "D:/Git/py-kitchen/apps/test-routines/oop.py", line 29, in <module>
Exiting
    print(9/0)
Exception happend  <class 'ZeroDivisionError'> division by zero <traceback object at 0x000001F94E5C4C48>
ZeroDivisionError: division by zero

```
So in the above case even if there was an exception in main program flow, `__exit__` in point object gets 
called and its aware of exception happend and handle its clean up irrespective of exception in main flow. 

#### Example
As an example let us create a Portfolio holding collection object. So this class loads all holdings
and provides a collection interface. The holding information is stored in a csv file.  

```
Name,Date,Shares,Price
HPQ,7/11/2007,100,32.2
IBM,7/12/2007,50,91.9
GE,7/13/2007,150,83.44
CAT,7/14/2007,200,51.23
MSFT,7/15/2007,95,40.37
HPE,7/16/2007,50,65.1
AFL,7/17/2007,100,70.44

```
As a first step, let us create our Holding class.  

```python
class Holding(object):
    def __init__(self, name, date, shares, price):
        self.name = name
        self.date = date
        self.shares = shares
        self.price = price

    def __str__(self):
        return 'Name = {}, Date={}, Shares={}, Price={}'.format(self.name,
                                                                self.date,
                                                                self.shares,
                                                                self.price) 

```

Now let us create our Portfolio collection object that can hold Holdings and provides
methods to access that. Here we will exercise our knowledge on some magic methods.  

<pre class='line-numbers'>
<code class='language-python'>
class Portfolio(object):
    def __init__(self):
        self.holdings = []

    @classmethod
    def load_from_csv(cls, filename):
        self = cls()
        with open(filename, 'r') as f:
            rows = csv.reader(f)
            head = next(rows) # skip header
            for row in rows:
                try:
                    self.holdings.append(Holding(name=row[0], date=row[1],
                                                 shares = int(row[2]), price=float(row[3])))

                except ValueError as ve:
                    print('ignored{}'.format(row))
        return self

    def __len__(self):
        return  len(self.holdings)

    def __getitem__(self, item):
        return self.holdings[item]

    def __iter__(self):
        return  self.holdings.__iter__()

    def cost(self):
        return sum([(holding.price * holding.shares) for holding in self.holdings])
</code>
</pre>

So first step, we created a factory method (@classmethod) which creates a Portfolio from csv file.(line:5).
Then we added some magic methods that will provide collection like properties to Portfolio. We have 
implemented `__len__, __getitem__, __iter__ `. In addition we have implemented a `cost` method that 
computes the value of the portfolio.  

```python
# program.py
portfolio = Portfolio.load_from_csv('stocks.csv')
for item in portfolio:
    print (item)

print ('Total number of holdings is {}'.format(len(portfolio)))

print('Total value of portfolio is {}'.format(portfolio.cost()))

```
outputs..

```bash
Name = HPQ, Date=7/11/2007, Shares=100, Price=32.2
Name = IBM, Date=7/12/2007, Shares=50, Price=91.9
Name = GE, Date=7/13/2007, Shares=150, Price=83.44
Name = CAT, Date=7/14/2007, Shares=200, Price=51.23
Name = MSFT, Date=7/15/2007, Shares=95, Price=40.37
Name = HPE, Date=7/16/2007, Shares=50, Price=65.1
Name = AFL, Date=7/17/2007, Shares=100, Price=70.44  

Total number of holdings is 7
Total value of portfolio is 44711.15

```

_Coding is fun enjoy..._  


