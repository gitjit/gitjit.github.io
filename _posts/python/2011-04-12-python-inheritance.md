---
layout: post
title:  "Python : Object Oriented Programming (Inheritance)"
description: "Understanding Inheritance in Python"
date:   2011-05-10
tags: [Python]
comments: true
references: [
   "Standard library : https://docs.python.org/3/library/index.html",
   "Dan Brader Channel : https://www.youtube.com/watch?v=PNpt7cFjGsM&t=681s&index=6&list=PLP8GkvaIxJP0VAXF3USi9U4JnpxUvQXHx",
]

excerpt: "In this post we will try to understand Inheritance in Python Object Oriented Programming. We will be learning
this by writing a simple table formatter, which generates tabular data from a collection of objects."
---

In this post we will try to understand Inheritance in Python Object Oriented Programming. We will be learning
this by writing a simple table formatter, which generates tabular data from a collection of objects..  

So our program will be reading csv data from a file shown below and represents it in a tabular format.

<pre class='line-numbers'>
<code class='language-bash'>
# stocks.csv
**Name,Date,Shares,Price**
HPQ,7/11/2007,100,32.2
IBM,7/12/2007,50,91.9
GE,7/13/2007,150,83.44
CAT,7/14/2007,200,51.23
MSFT,7/15/2007,95,40.37
HPE,7/16/2007,50,65.1
AFL,7/17/2007,100,70.44
</code>
</pre>

So as a first step, let us create a class to hold this info. 

```python
class Holding(object):
     def __init__(self,name, date, shares, price ):
         self.name = name
         self.date = date
         self.shares = shares
         self.price = price

```

Now let us write a function that can display any objects in a text tabular format.The input to this function are
list of objects and column names/attribute names of the objects. We are using `getattr` to get the attribute
value to be displayed. (Its important that column name should match attribute.You can improve this by passing a
tuple which hold display value and attribute value if we need to display diffrent column name that attribute name)

```python
def print_table(objects, columns):
    '''
    This method prints a table from objects using getattr
    :param objects: object list to iterate
    :param columns: column names matching attributes of object
    '''
    # print header
    for col in columns:
        print('{:>10s}'.format(col), end=' ')
    print()
    for obj in objects:
        for col in columns:
            print('{:>10s}'.
                  format(str(getattr(obj, col))), end=' ')
        print()

```

Now let us use the above method to generate table of 'stocks.csv'  First let us load that data into a 
list and then generate the table as shown below.

```python

# program.py
import csv
def load_portfolio():
    holdings = []
    with open('stocks.csv', 'r') as f:
        rows = csv.reader(f)
        head = next(rows)
        for row_no, row in enumerate(rows):

            try:
                holdings.append(Holding(name=row[0], date=row[1],
                                    shares=int(row[2]), price=float(row[3])))

            except ValueError as ve:
                print('Ignoring row {}-{}'.format(row_no,row))

    return holdings
# print the table
print_table(load_portfolio(), ['name','date', 'shares', 'price'] ):

```  

Now comes interesting part, what if we want to print as CSV, html tables etc.. So its good to define
a contract for the formatter and derive sub classess from it, the logic of formatting can be 
eliminated from the print_table().

<img src='/images/2017-04-25-17-36-00.png' class='img-responsive'>

So we can rewrite print_table() as follows.  As first step let us create a base class '`Formatter`',
which is like an interface that specifies the contract.  

```python
class Formatter(object):
    def print_header(self, cols):
        pass

    def print_records(self, objects):
        pass

```

Now let us modify our print_table() as follows.  

```python
def print_table(objects,columns, formatter):
    # print header
    formatter.print_header(columns)

    # print rows
    for obj in objects:
        formatter.print_records([str(getattr(obj, col)) 
                                 for col in columns])

```

Now let us implement our custom formatters. 

TextFormatter..

```python
class TextFormatter(Formatter):

    def print_header(self, headers):
        for head in headers:
            print('{:>10s}'.format(head), end=" ")
        print()

    def print_records(self, rowdata):
        for item in rowdata:
            print('{:>10s}'.format(item), end=" ")
        print()

``` 

CSVFormatter..

```python
class CSVFormatter(Formatter):
    def print_header(self, headers):
        print(','.join(headers))
        print()

    def print_records(self, rowdata):
        print(','.join(rowdata))
        print()

```

HTMLFormatter..

```python
class HTMLFormatter(Formatter):

    def print_header(self, headers):

        print('<tr>')
        for head in headers:
            print('<th>{}</th>'.format(head))
        print('</tr>')

    def print_records(self,rowdata):
        print('<tr>')
        for item in rowdata:
            print('<td>{}</td>'.format(item))
        print('</tr>')

```

Now we can use these formatters as shown below..

```
print_table(load_portfolio(), 
            ['name','date', 'shares', 'price'], 
            HTMLFormatter())

print_table(load_portfolio(), 
            ['name','date', 'shares', 'price'], 
            CSVFormatter())

print_table(load_portfolio(), 
            ['name','date', 'shares', 'price'], 
            TextFormatter())

```

output :

```
#html
<tr>
    <th>name</th>
    <th>date</th>
    <th>shares</th>
    <th>price</th>
</tr>
<tr>
    <td>HPQ</td>
    <td>7/11/2007</td>
    <td>100</td>
    <td>32.2</td>
</tr>
<tr>
    <td>IBM</td>
    <td>7/12/2007</td>
    <td>50</td>
    <td>91.9</td>
</tr>
<tr>
....
....

```


_Coding is fun enjoy..._  


