---
layout: post
title:  "Hacks/Tools"
description: "Some common hacks or tools useful for development."
date:   2017-10-01
tags: [Hacks/Tools]
comments: true
references: [
   "https://stackoverflow.com/questions/4895966/changing-default-startup-directory-for-command-prompt-in-windows-7",
   
]

excerpt: "This post contains a collection of hacks or tools useful for
development. It might be a snippet from stack over flow or from some other useful sites."
---

### How to change default startup directory for Windows terminal/console ?  

Go to registry (Type regedit from command prompt) and browse to following path  

```
HKEY_CURRENT_USER\Software\Microsoft\Command Processor

```
Now create a new string value named "Autorun" and set its value to your desired location as shown below. 
In this case I am setting this to "C:\1\".  

`cd /d C:\`

Disclaimer : Since you are editing registry, ensure to keep a copy. I have tested this only in Win-10 
and it should work fine in other windows version too.  









