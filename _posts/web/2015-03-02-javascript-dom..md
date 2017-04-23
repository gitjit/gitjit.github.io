---
layout: post
title: "Javascript : Objects and  Dom"
date: 2015-03-02
tags: [Javascript]
references: [
   "w3schools : https://www.w3schools.com/js/",
]

excerpt: "This post is about some Javascript basics. It covers some basics about Object, Object Methods and DOM in
Javascript. We will cover basics on following topics and will do a simple sample to solidify our concepts. "
---

Before starting our discussion on DOM(Document object model) in Javascript let us revise our understanding on `Objects` 
and `Object Methods` in Javascript. Objects in JS are similar to dictionaries in python. Object methods enables JS objects
to have a method embedded in it. Let us create a simple Employee object first.

```javascript
var emp =  {
    name: "Jack Robbins",
    age:41,
    job:"Actor"
};

```
You can access object members using corresponding keys.  

```javascript
>> emp['name']
"Jack Robbins"
>> emp['age']
41
>> emp['job']
"Actor"

```
Now let us add  a method which returns last name from the object.An object method can access variables in same object
using `this` keyword.

```javascript
var emp =  {
    name: "Jack Robbins",  
    age:41,  
    job:"Actor",  
	last_name: function(){  
		  var names = this.name.split(" ");
		  console.log(names[1]);  
    }};


```
output :  

```
emp.last_name()
VM172:9 Robbins

```  
Now having a basic idea on Javascript objects,let us start our discussion on `DOM`.`DOM ` allows to interface
our Javascript code to interact with HTML and CSS in the page.Browsers will construct the DOM, which basically
means storing all the HTML tags as Javascript objects. Using Javascript we can access this DOM and do processing
on these objects.  
We can see DOM of a webpage in console of browser by typing `>> document`,which will return entire html text of the
page. To see actual Javascript objects in DOM type `>>console.dir(document)`.  

Following are some important DOM attributes.

* **document.URL** (Get URL of the web page)
* **document.body** ( Get body element)
* **document.head**(Get html header)
* **document.links** (Get all hyperlinks in page)

Following are some commonly used methods for grabbing elements from DOM.  

* **document.getElementById()**
* **document.getElementByClassName()**
* **document.getElementsByTagName()** (returns array of all elements with that tag name)
* **document.querySelector()** (returns first element that matches tag/class/id)
* **document.querySelectorAll()**  (returns array of all elements that matches)  

### Sample : Header color changer

Let us do a simple color changer example to understand the concept. Let us create a simple html page with an `<h1> and <p>`  
Now open the page in a browser and type following code in console to change color of paragraph and header.  

``` html
var header = document.getElementsByTagName("h1")  
header[0].style.color = "Yellow"

var para = document.querySelector("P")  
para.style.color = "red"

```  
### Sample : Header random color changer  

Now let us move away from our console to a real Javascript file. In this example, we will be applying a Javascript to 
our html page, which applies a random color to our header element on constant intervals.  

```javascript
//color-change.js

var header = document.querySelector("h1");
setInterval(changeHeaderColor,500);

/*
    Change header color
*/
function changeHeaderColor()
{
    header.style.color = generateRandomColor();
}

/*
    Generate a random hex color
*/

function generateRandomColor()
{
    
    var letters = "123456789ABCDEF";
    var color = "#";
    for (var i=0; i<6; i++)
    {
        color += letters[Math.floor(Math.random()*16)] 
    }
   
    return color;
};

```
### Content,Attribute and HTML manipulation  

Now we will see more examples of how to manipulate content using Javascript.  We will see how to change text,
html code and attributes. We can use following DOM methods to manipulate the content and attributes..

var myElement = document.querySelector(""); # select the element

* myElement.textContent (get/set the text content on that element)
* myElement.innerHtml (get/set actual html)
* myElement.setAttribute() (set attribute)
* myElement.getAttribute() (get attribute)

Now let us apply this on an example. This is our html page.

```html
 <body>
       
       <h1 style="color:rebeccapurple">Color Changer</h1>

       <p class="lorem">Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
       
       <h3>This is a link to  <a href="http://amazon.com">Amazon</a>  </h3> 
    
    </body>

```
As a first step, let us change the content of the paragraph. 

```javascript
var para = document.querySelector(".lorem"); 
para.textContent="This is an new text...";  

```
Now let us make this content bold. For this we have to change the html itself with a strong tag. 

```javascript
para = document.querySelector("p")
para.innerHTML= "<strong>This is a strong text </strong>"

```
Now let us change h3 to point to Google instead of Amazon. 

```javascript
var h3header = document.querySelector("h3");
var amzlink = h3header.querySelector("a");
amzlink.getAttribute("href");
amzlink.setAttribute("href","http://www.google.com");
amzlink.textContent="Google";

```




_Coding is fun enjoy..._  