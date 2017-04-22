---
layout: post
title:  "Python : Packaging"
description: "Creating a package for python modules"
date:   2011-04-08
tags: [Python]
comments: true
source: "https://github.com/GitJit/py-kitchen/tree/master/apps/stocker"
references: [
  "Standard library : https://docs.python.org/3/library/index.html",
   "Davide Beazly Pycon : https://www.youtube.com/watch?v=0oTh1CXRaQ0&t=6080s",
]

excerpt: "This post explains how python packaging works and shows how simple it is organise modules
as packages. Python's approach of packaging is much neater and simpler than most of other programming
languages where Class Path is important. This post will provide necessary basics about how packaging
works in Python.
"
---  

This post explains how python packaging works and shows how simple it is organise modules
as packages. Python's approach of packaging is much neater and simpler than most of other programming
languages where Class Path is important. This post will provide necessary basics about how packaging
works in Python.

Any file with extension `.py` is a module and any folder that contains multiple modules is called a 
python package.  

In our previous [post](http://pepgin.com/csv-data-analysis/) we created two handy modules (`csvreader.py and portfolio.py`)
which can be packaged for reuse. With out packaging and using it as a top level module is a risk on bigger projects as it can create
name conflicts. So let us make these modules as part of a package called "stocker".

As a first step of packaging let us create a new folder called "stocker", it will be our package 
name. Let us copy `csvreader.py and portfolio.py` into this folder. In addition we need to create a 
file `__init__.py` inside the folder to mark it as a package.

<img src='/images/2017-04-21-16-58-26.png' class='img-responsive'>

Now let us try to access this package from another file `program.py `.

Let us start with `csvreader`  

<pre class='line-numbers'>
<code class='language-python'>

#program.py

import stocker.csvreader
print(stocker.csvreader.read_csv('stocks\stocks.csv',[str,str,int,float]))

</code>
</pre>

output..
 
`[{'Shares': 100, 'Date': '7/11/2007', 'Name': 'HPQ', 'Price': 32.2},
 {'Shares': 50, 'Date': '7/12/2007', 'Name': 'IBM', 'Price': 91.9}, 
 {'Shares': 150, 'Date': '7/13/2007', 'Name': 'GE', 'Price': 83.44}, 
 {'Shares': 200, 'Date': '7/14/2007', ...`

So it worked well !

Now let us try to use portfolio module to calculate top valued stock as shown below. Remember that
portfolio.csv internally imports csvreader which is also part of this package.  

<img src='/images/2017-04-21-12-42-16.png' class='img-responsive'>

<pre class='line-numbers'>
<code class='language-python'>

#program.py

import stocker.csvreader
import stocker.portfolio # this fails
print(stocker.csvreader.read_csv('stocks\stocks.csv',[str,str,int,float]))

</code>
</pre>

output..  
`import csvreader
ImportError: No module named 'csvreader'  
`

The issue happening here is, Python when importing portfolio is looking to load csvreader. Python
consider's csvreader as a toplevel module and looks for it in toplevel module path and fails. In 
order to solve this we have to specify from where to import csvreader. It can be resolved by updating
import statement of csvreader in porfolio.py as follows..

`from . import csvreader`

<pre class='line-numbers'>
<code class='language-python'>
# portfolio.py
import glob
import os
import csv
import json
import pprint
import requests
import itertools
from . import csvreader

def get_portfolio_as_dict(path):
    pattern = '*.csv'
    files = glob.glob(os.path.join(path,pattern))
    portfolios = []
    for file in files:
        for row in csvreader.read_csv(file,[str, str, int, float]):
            portfolios.append(row)
    return portfolios

</code>
</pre>

This is an important step to be taken when creating packages. Another advantage of using '.' is that
it is not tied to the package name. So even if package name changed it should work well. ! Now let us
dwell into another file we created `__init__.py`.  

`__init__.py' is called as package initializer and it gets executed before loading any modules in the 
package. So we can use `__init__.py` to do some initialization before any other modules in package
gets loaded. It is widely used to lift symbols from submodules up a level and abstract the implementation
details. So in our `__init__.py` let us lift symbols in modules in package as follows.  

<pre class='line-numbers'>
<code class='language-python'>
# __init__.py
from .csvreader import read_csv
from .portfolio import get_top_portfolio
</code>
</pre>

Now we can import the package and use `read_csv and get_top_portfolio` as shown below. So we can abstract
our core API's behind the package name.  

<pre class='line-numbers'>
<code class='language-python'>
# program.py

import stocker
print(stocker.read_csv('stocks\stocks.csv',[str,str,int,float]))

print(stocker.get_top_portfolio('stocks'))

</code>
</pre>


_Coding is fun enjoy..._  
