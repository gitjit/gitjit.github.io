---
layout: post
title:  "Hacks"
description: "Some common hacks or tools useful for development."
date:   2017-02-01
tags: [.Net,C#]
comments: true
references: [
   "MSDN:https://msdn.microsoft.com/en-us/library/z1zx9t92.aspx",
   "C# Corner:http://www.c-sharpcorner.com/article/assembly-in-net/",
   
]

excerpt: "This post contains a collection of hacks or will introduce a set of tools useful for
development. It might be a snippet from stack over flow or from some other useful sites."
---

### How to change default startup directory for Windows terminal/console ?  

Go to registry (Type regedit from command prompt) and browse to following path  

```
HKEY_CURRENT_USER\Software\Microsoft\Command Processor

```
Now create a new string value name "Autorun" and set its value to your desired location as shown below. 
In this case I am setting this to "C:\1\".  

`cd /d C:\`





