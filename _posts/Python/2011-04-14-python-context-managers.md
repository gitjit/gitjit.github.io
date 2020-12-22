---
layout: post
title:  "Python : Custom Context Managers"
description: "Custom Context Managers in Python"
date:   2011-04-14
tags: [Python]
comments: true
references: [
   "Standard library : https://docs.python.org/3/library/index.html",
   "Python course : http://www.python-course.eu/python3_inheritance.php",

]

excerpt: "In this post we will discuss some internals of Context Managers in Python and how to leverage that in our class, if we have to deal with resource management"
---

Operating System resources must be release after our use. Some common resource handles are file, sockets, thread locks etc. So if our program is using these resources we need to ensure that we are releasing it after our use. A general pattern of handling resource is like this. 

<pre class='line-numbers'>
<code class='language-python'>
    def read_file():
        f = open(filename, 'r')
        data = f.read()
        f.close()
</code>
</pre>

What will happen if there was an exception in between and f.close() never happens ?  That means we are not properly cleaning the resource handle. Context managers helps us to do this in a clean way as shown below.  

<pre class='line-numbers'>
<code class='language-python'>
    def read_file():
       with open(filename, 'r') as f:
            data = f.read()
            f.close()
</code>
</pre>

In this case its guarantee that the file handle gets released. Let us see how context managers works.




_Coding is fun enjoy..._  


