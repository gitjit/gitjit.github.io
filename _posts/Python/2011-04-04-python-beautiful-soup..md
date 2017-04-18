---
layout: post
title:  "Python : Using Beautiful Soup"
description: "A simple weather app to illustrate Beautiful Soup"
date:   2011-04-04
tags: [Python]
comments: true
source: "https://github.com/GitJit/python-core/tree/master/apps/07.1-wizard_battle"
references: [
   "Beautiful Soup documentation : https://www.crummy.com/software/BeautifulSoup/bs4/doc/",
   "Requests module : http://docs.python-requests.org/en/master/",
   "Automate boring stuff : https://automatetheboringstuff.com/",
   
]

excerpt: "This post is about using `Beautiful` Soup and `requests` module in python application for webscrapping. 
We are writing a simple weather application that uses Beautiful Soup to scrap the web and uses request module to download
from web to display weather at a particular zip code. After reading this post you will get a basic idea on 
how to use `Beautiful Soup` and `requests` module in python."
---  

This post is about using `Beautiful` Soup and `requests` module in python application for webscrapping. 
We are writing a simple weather application that uses Beautiful Soup to scrap the web and uses request module to download
from web to display weather at a particular zip code. After reading this post you will get a basic idea on 
how to use `Beautiful Soup` and `requests` module in python.  

Note :_I have recently updated this post to use BS4 and requests_. 

Beautiful Soup sits atop an HTML or XML parser, providing Pythonic idioms for iterating, searching, and modifying the parse tree.
Requests is a module that makes sending http requests simple.So first step is to include these packages as part of our project and 
using it. We will use `pip` to install the packages.

<pre class='line-numbers'>
<code class='language-bash'>
    pip install beautifulsoup4    
    pip install requests
</code>
</pre>  

<pre class='line-numbers'>
<code class='language-python'>
import requests
from bs4 import BeautifulSoup
import  collections

</code>
</pre>

We will be using [weather-underground](https://www.wunderground.com) to determine the conditions at a particular location.
For the sake of our application goal, instead of using their web service we will be downloading the html and parsing that to 
determine the weather. If you pass zip code along with a base url, weather underground will display weather at that location 
as shown below.  

<img src='/images/2017-04-18-15-48-39.png' class='img-responsive'>

As a first step, let us get the zip code from user. 

<pre class='line-numbers'>
<code class='language-python'>
def get_zip():
    zip_code = input("Enter your zip code : ")
    return  zip_code
</code>
</pre>

Now download the html from url using `requests` and parse it using BS4.

<pre class='line-numbers'>
<code class='language-python'>

def get_url_html(url):
    response = requests.get(url)
    # response.status_code 200 OK
    # returns html
    return response.text

</code>
</pre>

I uses Chrome developer tool to inspect the html source and determine whilch html element has required info. First item I am trying 
to read is the city name and it resides in an `H1` tag inside a div with `id="location"`. In a similar way 'condition' is in a div
 with `id='curCond'` and `class = 'wx-value'` and so on.. So based on html and css info let us grab required information from the web page.
 
<pre class='line-numbers'>
<code class='language-python'>
def get_weather_from_html(html):
    soup = BeautifulSoup(html,'html.parser')
    loc = soup.find(id='location').find('h1').get_text().strip()
    loc = get_city(loc)
    condition = soup.find(id='curCond').find(class_ ='wx-value').get_text()
    condition = clean_text(condition)
    temp = soup.find(id='curTemp').find(class_ ='wx-data').find(class_ = 'wx-value').get_text()
    temp = clean_text(temp)
    unit = soup.find(id='curTemp').find(class_ ='wx-data').find(class_ = 'wx-unit').get_text()
    unit = clean_text(unit)
    # print("Weather @ {0} : Temperature = {1}{2} {3}".format(loc, temp, unit, condition))
    # return named tuple
    w_report = WeatherReport(location=loc, temperature=temp, scale=unit, cond=condition)
    return w_report
</code>
</pre>

Above function returns results as a named tuple, which is easy when we are passing large result as a tuple. 
This is how we create a `namedtuple`. It is part of collections module. 

<pre class='line-numbers'>
<code class='language-python'>
def clean_text(text: str):
    if not text:
        return text
    return text.strip() 

def get_city(text: str):
    if not text:
        return text
    text = clean_text(text)
    parts = text.split('\n')
    return parts[0]
</code>
</pre>

Its also important that results might need some clean up. For example city name we got back is 
having new line character and we need to do some clean up there.

<pre class='line-numbers'>
<code class='language-python'>
# create a named tuple for returning multiple values
WeatherReport = collections.namedtuple('WeatherReport', 'location, temperature, scale, cond')
</code>
</pre>

Given below is the `__main__` entry.  

<pre class='line-numbers'>
<code class='language-python'>
if __name__ == '__main__':
    print_header()
    zip_code = input("Enter your zip : ")
    url = base_url+'/'+zip_code
    html = get_url_html(url)
    w_report = get_weather_from_html(html)
    print("The weather in {} is {}{} and {}".format(w_report.location, w_report.temperature,
                                                    w_report.scale, w_report.cond))
</code>
</pre>

##### Result  
<pre class='line-numbers'>
<code class='language-bash'>
--------------------------------------
            Weather App
--------------------------------------

Enter your zip : 92127
The weather in San Diego, CA is 75.8°F and Clear

Process finished with exit code 0
</code>
</pre>


_Coding is fun enjoy..._  

