---
layout: post
title:  "Regular Expressions"
date:   2015-09-15 09:45:36
categories: Python
author : Jithesh Chandrasekharan
---

Regular expressions are text matching patterns described with a formal syntax. You’ll often hear regular expressions referred to as ‘regex’ or ‘regexp’ in conversation. Regular expressions can include a variety of rules, from finding repetition, to text-matching, and much more. If you’re familiar with Perl, you’ll notice that the syntax for regular expressions are very similar in Python. We will be using the re module with Python here.

**Pattern Search in Text**
One of the most common uses for the re module is for finding patterns in text.

{% highlight python %}

phrase = '''This is to test for term1 in a 
phrase using regular expression.
term is the word we search for match.'''
 
for term in terms:
    match = re.search(term,phrase)
    if match:
        print 'Found ' + term + ' starts at ' + 
        str(match.start()) + 'ends at ' + str(match.end())
    else:
        print 'Not found ' + term
 
Output:
Found term1 starts at 20 ends at 25
Not found term2

{% endhighlight %}

**Split**
Another handy feature is to split a phrase based on regular expression.

{% highlight python %}
email = 'redflix@gmail.com'
output = re.split('@', email)
print 'Name is '+ output[0] + ' and domain is ' + output[1]
 
output:
Name is redflix and domain is gmail.com
{% endhighlight %}

**FindAll**
Is used to find all matching instances of a pattern.
{% highlight python %}
phrase = 'find all instance of a pattern. An object is an instance of class'
re.findall('instance', phrase)

output:
['instance', 'instance']
{% endhighlight %}

**re.compile(pattern,flags=0)**
It is common among programmers to use compile which Compile a regular expression pattern into a regular expression object, which can be used for matching using its match() and search() … methods.

{% highlight python %}
reg_ex = re.compile(pattern)
result = reg_ex.match(string)
{% endhighlight %}

<iframe width="640" height="360" src="https://www.youtube.com/embed/C-JauEnlSlM" frameborder="0" allowfullscreen></iframe>



