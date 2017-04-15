---
layout: post
title: "Bootstrap and Jekyll"
date: 2017-03-02
tags: [jekyll]
source: "https://github.com/GitJit/hadoop_kitchen/tree/master/Avro"
references: [
   "PrideParrot:http://prideparrot.com/blog/archive/2014/4/blog_template_using_twitter_bootstrap3_part1",
   "Nikitha's blog:https://nikinath.github.io/", 
   "Jekyll guide:https://jekyllrb.com/docs/quickstart/",  
]

excerpt: "This week I completely redesigned my blog from a non-responsive blog to a responsive blog using Bootstrap. In my previous version I was primarily using 
a default theme from Jekyll documentation. In this post I will share the details of steps I did to redesign my blog to a responsive one. I started creating the 
default layout out page using Bootstrap."  
---

This week I completely redesigned my blog from a non-responsive blog to a responsive blog using Bootstrap. In my previous version I was primarily using 
a default theme from Jekyll documentation. In this post I will share the details of steps I did to redesign my blog to a responsive one. I started creating the 
default layout out page using Bootstrap.  

As a first step, downloaded Bootstrap-3.3.7 and Jquery-3.2.0.min.js and included in my project. The version numbers are not important I just downloaded the latest 
versions. Now updated by default.html layout file with appropriate entries for Bootstrap and Jquery.  On a highlevel the markdown will looks like this and as
this post progresses we will fill necessary pieces.

```html
<body>
    <header>
        <div class="container">
            -- Logo, Navigation & Search Bar ï¿½-
        </div>
    </header>
 
    <div id="body" class="container">
        -- Site Main content --
    </div>
 
    <footer>
        <div class="container">
            -- Copyright information ï¿½
        </div>
    </footer>
</body>

```

#### Navigation Bar 
First improvement was to come up with a responsive navigation bar. Class 'navbar-inverse' basically inversed the color of 
navigation bar. The sandwich button which will be visible on smaller screens are created using 'icon-bar' spans as shown above.
'navbar-brand' is used to decorate the brand name. The connection between the toggle button for small screens and menu items
are connected through 'bs-navbar-collapse'. The date target 'data-target=".bs-navbar-collapse' is specified in sandwich button.
I think remaining decorators in navigation bar html is self explanatory. The entire menu is re-used across multiple highlevel
pages, so the entire html is defined  in a file 'menu.html'

```html 
<header class="navbar navbar-inverse navbar-fixed-top bs-docs-nav" role="banner">
    <div class="container">
        <!--toggle button visible in mobile-->
        <div class="navbar-header">
            <button class="navbar-toggle" type="button" data-toggle="collapse" 
             data-target=".bs-navbar-collapse">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a href="{{site.baseurl}}/index.html" class="navbar-brand">DATA-TRICKS</a>
        </div>
        <!--menu list items-->
        <nav class="collapse navbar-collapse bs-navbar-collapse" role="navigation">
             
            <form class="navbar-form navbar-right" role="search">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">Submit</button>
            </form>
            <ul class="nav navbar-nav ">
                <!--<li class="active"><a href="{{site.baseurl}}/index.html">Home</a></li>-->
                <!--<li><a href="contact.html">Contact</a></li>-->
                <li><a href="{{site.baseurl}}/about/">About</a></li>
            </ul>
           
        </nav>
    </div>
</header>

```  
We can divide the markup (listing 10) into two sections: header (div.navbar-header) and navigation (div.navbar-collapse).
The header section contains the trigger button which is used to invoke the navigation to expand or collapse in the mobile screen. 
The navigation section contains the unordered list of links which we saw earlier.

These two sections behave differently in case of desktop and mobile views. In case of desktop screen, the button is hidden
and the navigation section is displayed. While in case of mobile view, the button is shown and navigation section is hidden as 
default. When you click the button the navigation shows up.

### Body  







Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.
<pre><code class="language-css">p { color: red }</code></pre>
Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum. 


![My dog](/images/underconstruction.jpg)
  

So a  cosine similarity of ~ 1 represents that the vectors are pointing in the same direction and very similar.

{% highlight python %}
import math
from collections import Counter

def create_vector(iterable1, iterable2):
    counter1 = Counter(iterable1)
    counter2 = Counter(iterable2)
    all_items = set(counter1.keys()).union(set(counter2.keys()))
    vector1 = [counter1[k] for k in all_items]
    vector2 = [counter2[k] for k in all_items]
    return vector1, vector2

def calculate_cosim(v1, v2):
    dot_product = sum(n1 * n2 for n1, n2 in zip(v1, v2) )
    magnitude1 = math.sqrt(sum(n ** 2 for n in v1))
    magnitude2 = math.sqrt(sum(n ** 2 for n in v2))
    return dot_product / (magnitude1 * magnitude2)


l1 = "I love doing marketing and sales. A good salesman should be always punctual".split()

l2 = " He is a very good programmer. Bad programmers have poor math foundation".split()
    
v1, v2 = build_vector(l1, l2)
print(calculate_cosim(v1, v2))

Output : 0.0800640769025
{% endhighlight %}

So the above output shows that documents are not similar. So now let us apply another example on the above program.  This shows that two docs are close enough.

~~~~~~
l1 = "In football with penalty you can score a goal that can change the game. what a game. Players are dancing ".split()

l2 = "Its a penalty goal .In football a single score can change game. what a game. Players are dancing".split()
    

v1, v2 = build_vector(l1, l2)
print(calculate_cosim(v1, v2))
    

Output : 0.809
~~~~~~~

Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.