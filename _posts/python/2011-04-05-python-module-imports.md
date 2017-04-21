---
layout: post
title:  "Python : Modules,Imports and Packages"
description: "Understanding module imports in Python"
date:   2011-04-05
tags: [Python]
comments: true
references: [
   "Standard library : https://docs.python.org/3/library/index.html",
   "Modules : https://docs.python.org/3/tutorial/modules.html",
]

excerpt: "This post is about modules and imports in Python. As program grows we will split into several files
for easy maintainance and re-use.These files are called modules. For using functions in these modules we have to import module into
other modules or main module. This post discusses about modules and imports. After reading this post you will get a basic understanding
on how modules and imports works in python"
---  

This post is about modules and imports in Python. As program grows we will split into several files
for easy maintainance and re-use.These files are called modules. For using functions in these modules we have to import module into
other modules or main module. This post discusses about modules and imports. After reading this post you will get a basic understanding
on how modules and imports works in python.  

Let us create a module (calk.py) for advancing our discussion, this is just a module with no real meaning or functionality .

<pre class='line-numbers'>
<code class='language-python'>
# calk.py
pi = 3.14

def add(x, y):
    return x+y

def mult(x, y):
    return x*y

def spam():
    add(pi, 4)
    print('junk')
    print(pi)
print ('started..')
spam()
</code>
</pre>

We can run this module directly as shown below.

<pre class='line-numbers'>
<code class='language-bash'>
>python calk.py
started..
junk
3.14
</code>
</pre>

In order to use this module we have to import it as shown below. Here we are using it from shell.
Importing a module loads the module into memory and creates an isoloated environment.For accessing
the module functions we uses the module name as a prefix.  

<pre class='line-numbers'>
<code class='language-bash'>
>>> import calk
started..
junk
3.14
>>> calk
<module 'calk' from 'D:\\Git\\py-kitchen\\apps\\test-routines\\calk.py'>
>>> calk.add(1,2)
3
</code>
</pre>

There is also another syntax to import as shown below.  

<pre class='line-numbers'>
<code class='language-bash'>
>>> from calk import add
>>> add(1,3)
4
>>> mult(3,4)
NameError: name 'mult' is not defined   

</code>
</pre>  

_There is a confusion that importing the module is this way is more efficient, but that is completely wrong.Both syntax
will load the entire module into memory and only thing the second syntax is doing is aliasing the specified function to
access with out module prefix_

Another important aspect regarding `import` is that its a one time operation in a session. So once a module is imported, 
its cached in memory of that session and further imports are ignored by python interpreter. You can view all modules cached
using sys module function as shown below.
.

<pre class='line-numbers'>
<code class='language-bash'>
>>> import sys
>>> sys.modules # print entire dictionary
>>> sys.modules['calk']
<module 'calk' from 'D:\\Git\\py-kitchen\\apps\\test-routines\\calk.py'>
</code>
</pre>  

That means, if we made any code changes in module and imported again with out exiting sessions, your recent code change in module
will not get reflected until you exit the session and import it again.  

Another info regarding `import` is that, sometimes when we try to import a module residing in a diffrent folder,
python fails to import that module. Because python checks only specific paths to find the modules .So if you are
trying to import a module outside this path you will have to augment the search path. You can see the search paths by running
following command.

`>>> sys.path`

You can use `sys.path.append()` or env variables to augment the path. 

In the above example when we import the module, it is getting executed as a script, but usually we 
don't want the module to be executed when we import it and execute only when we are running it as script.
We can use the magic variable `__name__ == '__main__'` to detect in which mode its running. So if its `__main__` it means
that its getting run as a script.  So let us update our module as shown below.

<pre class='line-numbers'>
<code class='language-python'>
pi = 3.14

def add(x, y):
    return x+y

def mult(x, y):
    return x*y

def spam():
    add(pi, 4)
    print('junk')
    print(pi)

if __name__ == '__main__':
    print('started..')
    print (__name__)
    spam()

</code>
</pre>
 
_Coding is fun enjoy..._  
