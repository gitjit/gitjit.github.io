---
layout: post
title: "Bootstrap : Fundamentals"
date: 2015-03-01
tags: [Web]
source: "https://github.com/GitJit/web-kitchen/tree/master/bootstrap/koffee-site"
references: [
   "Bootstrap grid system:http://getbootstrap.com/css/#grid",
   "Bootstrap forms:http://getbootstrap.com/css/#forms", 
   "Bootstrap navbar:http://getbootstrap.com/components/#navbar",
   
]

excerpt: "This post discusses some Bootstrap basics that will give a jump start to design beautiful web sites
using Bootstrap.As a part of this we will be creating a simple webpage that will cover topics like Navbar, Jumbotrons,
Grids, Thumbnails and Forms in Bootstrap "
---

This post discusses some Bootstrap basics that will give a jump start to design beautiful web sites
using Bootstrap.As a part of this we will be creating a simple webpage that will cover topics like Navbar,
Jumbotrons,Grids, Thumbnails and Forms in Bootstrap.Once you have an understanding of Bootstrap layouts, 
its all about how to reference the documentation and use it. There is no need to remember all bootstrap
classes.

Given below is the screen shot of site we are designing.

<img src='/images/2017-04-21-23-16-36.png' class='img-responsive'>  

So let us start with a blank html page and slowly add each components.  As a first step link the 
page with Bootstrap CDN.

```html
<head>
    <title></title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" 
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
    <link rel="stylesheet" href="style.css">
</head>
    
```
After linking with Bootstrap, its time to add Navbar. Go to bootsrap documentation -> components - > Navbar
and copy past the following code. 

```html
<body>
    <nav class="navbar navbar-default navbar-inverse">
        <div class="container">
            <div class="navbar-header">
                <a class="navbar-brand" href="#"> Koffee </a>
            </div>
            <ul class="nav navbar-nav">
                <li class="active"><a href="sign.html">Sign In</a></li>
            </ul>
        </div>
    </nav>
 </body>

```
One important class we used here is `<div class="container">`. The container class ensures that the content in
body is centered and responsive.Containers are the most basic layout element in Bootstrap and are required 
when using default grid system. Choose from a responsive, fixed-width container (meaning its max-width changes at each breakpoint) or
fluid-width (meaning it’s 100% wide all the time).

Now let us add the jumbotron and paragraphs, as mentioned above all are wrapped in `<div class="container">`.      

``` html
 <nav class="navbar navbar-default navbar-inverse">
        <div class="container">
            <div class="navbar-header">
                <a class="navbar-brand" href="#"> Koffee </a>
            </div>
            <ul class="nav navbar-nav">
                <li class="active"><a href="sign.html">Sign In</a></li>
            </ul>
        </div>
    </nav>

    <div class="container">
        <div class="jumbotron">
            <h1>Choose your Coffee!</h1>
            <p>This site is all about Coffee !</p>
            <p>Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's
                standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it
                to make a type specimen book. It has survived not only five centuries, but also the leap into electronic
                typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset
                sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker
                including versions of Lorem Ipsum.</p>
        </div>
    </div>
    <div class="container">
        <p>Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard
            dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type
            specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining
            essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum
            passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem
            Ipsum.
        </p>

        <p>Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard
            dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type
            specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining
            essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum
            passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem
            Ipsum.
        </p>
    </div>

```  
Now let us add image thumbnails . Before doing that let us understand the Grid system in bootstrap.Bootstrap includes a responsive, 
mobile first fluid grid system that appropriately scales up to 12 columns as the device or viewport size increases. It includes 
predefined classes for easy layout options, as well as powerful mixins for generating more semantic layouts. So how the contents
in a grid gets displayed is decided by the grid system classes.  

Bootstrap grid system splits the entire screen (any devices) into 12 available columns.We can use any combination of numbers that will
eventually add up to 12 columns.

<img src='/images/2017-04-22-07-54-06.png' class='img-responsive'>  

Grid system in bootstrap makes use of `class='row'`. This is the top level class that creates a grid system of 12 columns. Then inside
this we use specific classes to denote how many column that particular element takes up. This class has a generic format  
`.col-screensize-numberofcolumns` . Given below are the screen sizes.  

* Extra small devices Phones (<768px) - xs
* Small devices Tablets (≥768px) - sm
* Medium devices Desktops (≥992px) - md
* Large devices Desktops (≥1200px) - lg  

.col-md-4 means in a medium screen size this element will takes up 4 columns
.col-sm-6 means in a small screen size this element will takes up 6 columns.  

Another important point to remember is that each columns are nested meaning we can further split each columns to another 12 columns.
So with this knowledge we can create our thumbnails to show coffee pictures. 

```html
 <div class="container">
        <h1>Pictures of Coffee</h1>
        <div class="row">
            <div class="col-lg-4 col-sm-6 thumbnail"><img src="https://d2lm6fxwu08ot6.cloudfront.net/img-thumbs/960w/04LDEYRW59.jpg" alt=""></div>
            <div class="col-lg-4 col-sm-6 thumbnail"><img src="https://d2lm6fxwu08ot6.cloudfront.net/img-thumbs/960w/90V03Q5Y60.jpg" alt=""></div>
            <div class="col-lg-4 col-sm-6 thumbnail"><img src="https://d2lm6fxwu08ot6.cloudfront.net/img-thumbs/960w/O83SF2RB6D.jpg" alt=""></div>
            <div class="col-lg-4 col-sm-6 thumbnail"><img src="https://d2lm6fxwu08ot6.cloudfront.net/img-thumbs/960w/04LDEYRW59.jpg" alt=""></div>
            <div class="col-lg-4 col-sm-6 thumbnail"><img src="https://d2lm6fxwu08ot6.cloudfront.net/img-thumbs/960w/90V03Q5Y60.jpg" alt=""></div>
            <div class="col-lg-4 col-sm-6 thumbnail"><img src="https://d2lm6fxwu08ot6.cloudfront.net/img-thumbs/960w/O83SF2RB6D.jpg" alt=""></div>
        </div>

    </div>

```
`<div class="col-lg-4 col-sm-6 thumbnail">..</div>` is saying this div will take 4 columns of screen out of 12 columns in a large screen and 
in a small screen it will take up 6 columns. That is responsiveness. So our coffee thumbnails will be 3 thumbnails in a row in large/medium screen
and 2 thumbnails a row in small screens.   
Now let us design our sign up page.  

```html
<body>

    <div class="container">
        <div class="jumbotron">
            <h1>Log In</h1>
            <p>Enter your email and password </p>
        </div>
    </div>

    <div class="container">
    <form>
        <div class="form-group">
            <label for="exampleInputEmail1">Email address</label>
            <input type="email" class="form-control" id="exampleInputEmail1" placeholder="Email">
        </div>
        <div class="form-group">
            <label for="exampleInputPassword1">Password</label>
            <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
        </div>
        <div class="checkbox">
        <label>
        <input type="checkbox"> keep me signed in 
        </label>
  </div>
        <button type="submit" class="btn btn-default">Submit</button>
    </form>
    </div>
</body>

```
Now we cover basics of Bootstrap, which will help us to refer documentation and design our web pages.Its not easy to remember all
Bootstrap classes and even experts always refer documentation for designing web pages.

_Coding is fun enjoy..._  