---
layout: post
title:  "Visual Studio Code : Snippets"
description: "Adding custome snippets in VS Code"
date:   2017-04-08
tags: [Hacks/Tools]
comments: true
references: [
   "VSCode Youtube : https://www.youtube.com/watch?v=tBTISbnThTo",
   "VSCode docs : https://code.visualstudio.com/docs/editor/userdefinedsnippets",
]

excerpt: "This post explains how to add custom snippets in Visual Studio Code.Its always handy to use snippets 
          when ever possible. Snippets saves time and reduces syntax errors.This blog explains how to add a 
          custom snippet in mark down files, but concept is same for all other file types."
---

This post explains how to add custom snippets in Visual Studio Code.Its always handy to use snippets 
when ever possible. Code snippets saves time and reduces syntax errors.I am using [Prism.js](http://prismjs.com/)
in my blog for syntax highlighting. Code block insertion in prism is a pain when you have to specify line numbers
and language class. So I decided to create a snippet instead of typing the code block all times. 
Given below is the code block syntax for prism,where language varies.

![](..\..\images\2017-04-18-11-47-59.png)

In order to add custom snippets in VSCode go to `File -> Preferences - > User Snippets` and select the 
file type where you want to add snippet. In this case I am using markdown. All snippets are in json file
corresponding to file type. So in this case its markdown.json.  I have added a new snippet for above 
code block insertion as shown below. '$' represents tab numbers and it starts with 1.

![](..\..\images\2017-04-18-11-52-46.png)

So we start by giving a name for the snippet, in this case I gave the name 'Prism Code Block'. Prefix
is what we type to add snippet as part of auto complete. So in my case its 'prism_block' and body 
contains the actual code block to be inserted as part of snippet with place holders denoted by '$'.

Now you are ready to insert this snippet into your mark down files. Go to desired mark down and type 'prism_'
and auto complete will insert the code block for you. 

<img src='/images/2017-04-18-12-01-27.png' class='img-responsive'>

Coding is fun enjoy....






