---
layout: post
title:  "Python : Getattr"
description: "A simple use case of GetAttribute method in objects"
date:   2011-04-10
tags: [Python]
comments: true
source: "https://github.com/GitJit/py-kitchen/tree/master/apps/11.2-stock-calc-oops"
references: [
   "Standard library : https://docs.python.org/3/library/index.html",
   "GetAttribute : https://docs.python.org/3/library/functions.html#getattr",
]

excerpt: "In this post we will dicuss one use case of the built-in method getattr. We will develop a generic function that
uses getattr to print a tabular report from object attributes. By the end of this post you will get a better understanding
of getattr."
---
In this post we will dicuss one use case of the built-in method getattr. We will develop a generic function that
uses getattr to print a tabular report from object attributes. By the end of this post you will get a better understanding
of getattr.  

#### getattr(object, name[, default])  
Return the value of the named attribute of object. name must be a string. If the string is the name of one of the
object’s attributes, the result is the value of that attribute. For example, getattr(x, 'foobar') is equivalent to x.foobar.
If the named attribute does not exist, default is returned if provided, otherwise AttributeError is raised.
 
As a first step let us write a simple class `Holding` that can hold content from this sample portfolio
files.

<pre class='line-numbers'>
<code class='language-bash'>
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

<pre class='line-numbers'>
<code class='language-python'>
class Holding(object):
    def __init__(self, name, date, shares, price):
        self.name = name
        self.date = date
        self.shares = shares
        self.price = price
</code>
</pre>

Now let us create another class that can load these portfolio csv's and generate a collection of `Holdings`.  

```python
class Holdings(object):
    '''
        This class creates a collection of holding object from
        a list of *.csv files. Input param is path to a
        folder containing all files or a single file path.
    '''

    def __init__(self, path):
        self.path = path
        self.portfolios = []
        self.load()

    def load(self):
        files = []
        if os.path.isdir(self.path):
            files = glob.glob(os.path.join(self.path,'*.csv'))
        else:
            files.append(self.path)

        for file in files:
            with open(file, 'r') as f:
                rows = csv.reader(f)
                head = next(rows)
                for row_no , row in enumerate(rows):
                    try:
                        self.portfolios.append(
                            Holding(name=row[0],
                                    date=row[1],
                                    shares=int(row[2]),
                                    price=float(row[3])))
                    except ValueError as ve:
                        print('Ignoring {} {} in {} due to {}'.
                              format(row_no, row, file, ve))

```  
Now let us write a generic tabular printing function. The input to this function are collection of objects
and attribute names to be printed. 

```python
def print_objects(objects, columns):
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

```python
#program.py
if __name__ == '__main__':
    hld = Holdings('stocks')
    print_objects(hld.portfolios, 
                ['name', 'date', 'shares', 'price'])

```
output:
```bash
  name       date     shares      price 
       HPQ  7/11/2007        100       32.2 
       IBM  7/12/2007         50       91.9 
        GE  7/13/2007        150      83.44 
       CAT  7/14/2007        200      51.23 
      MSFT  7/15/2007         95      40.37 
       HPE  7/16/2007         50       65.1 
       AFL  7/17/2007        100      70.44 
     GOOGL  7/11/2008        100       55.2 
       IBM  7/12/2008         50       91.9 
        GE  7/13/2008        150      83.44 
       CAT  7/14/2008        200      51.23 

```


_Coding is fun enjoy..._  


