---
layout: post
title:  "Regular Expressions"
date:   2015-9-15 10:45:36
categories: Python
author : Jithesh Chandrasekharan
image: 
comments: true
---

Regular expressions are text matching patterns described with a formal syntax. You’ll often hear regular expressions referred to as ‘regex’ or ‘regexp’ in conversation. Regular expressions can include a variety of rules, from finding repetition, to text-matching, and much more. If you’re familiar with Perl, you’ll notice that the syntax for regular expressions are very similar in Python. We will be using the re module with Python here. Regular expression is a vast topic and cannot be covered in a single post. So in this post, I am trying to cover some of the most important aspects of regular expression a programmer needs. 

**Pattern search in text**
Regular expressions can be used for easy searching of a pattern inside a text. In the example below, we are searching for ['term1','term2'] in a particual phrase. In python you should import the module 're' for using regular expression in your script.

{% highlight python %}

import re
phrase = 'This is to test for term1 in a phrase using \
          regular expression.term is the word we search for match.'
    
terms = ['term2', 'term1']
for term in terms:
    match = re.search(term,phrase)
    if match:
        print 'Found ' + term + ' starts at ' + str(match.start()) + \
        'ends at ' + str(match.end())
    else:
        print 'Not found ' + term

Output:
Not found term2
Found term1 starts at 20ends at 25

{% endhighlight %}

**Split**
We can use split method to split a phrase based on a term.

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
It is common among programmers to use compile method which compiles a regular expression pattern into a regular expression object, which can be used for matching using its match(),search() … methods.

reg_ex = re.compile(pattern)
result = reg_ex.match(string)

**Using Metacharacters**
We can use metacharacters along with re to find specific types of patterns.

‘Ab*’ : This pattern will search for word in a phrase where A followed by zero or mord b’s

{% highlight python %}
phrase = 'Abamperes Abbbaar Aster'
reg_exp = re.compile('Ab*')
reg_exp.findall(phrase)

output:
['Ab', 'Abbb', 'A']

{% endhighlight %}

+ represents followed by 1 or more 

‘Ab+’ : This pattern will search for word in a phrase where A followed by one or mord b’s

{% highlight python %}
phrase = 'Abamperes Abbbaar Aster aback'
reg_exp = re.compile('Ab+')
reg_exp.findall(phrase)

output:
['Ab', 'Abbb']
{% endhighlight %}

'?' represents following by zero or 1 b 

‘Ab?’ : This pattern will search for word in a phrase where A followed by zero or exactly 1 b.

{% highlight python %}

phrase = 'Abamperes Abbbaar Aster aback'
reg_exp = re.compile('Ab?')
reg_exp.findall(phrase)

output:
['Ab', 'Ab', 'A']

{% endhighlight %}

{n} represents followed by ‘n’ b’s
‘Ab{3}’ : This pattern will search for words starting with A and followed by 3 b’s

{% highlight python %}
phrase = 'Abamperes Abbbaar Aster aback'
reg_exp = re.compile('Ab{3}')
reg_exp.findall(phrase)

output:
['Abbb']

{% endhighlight %}

{n,m} represents followed by n or m b’s

{% highlight python %}
phrase = 'Abamperes Abbbaar Abba abback'
reg_exp = re.compile('Ab{2,3}')
reg_exp.findall(phrase)

output:
['Abbb', 'Abb']
{% endhighlight %}

**Exclusions**
We can use ^ to exclude terms by incorporating it into the bracket syntax notation. For example: [^…] will match any single character not in the brackets. Let’s see some examples:

{% highlight python %}
phrase = 'This is a string! But it has punctutation. How can we remove it?'
reg_exp = re.compile('[^!. ]+')
reg_exp.findall(phrase)

output:
['This', 'is', 'a', 'string', 'But', 'it', 'has', 'punctutation', 
'How', 'can', 'we', 'remove', 'it?']

{% endhighlight %}
So in the example above, we have also included a white space too.  + indicates one or more of this repeating should be excluded.

**Character Ranges**

As character sets grow larger, typing every character that should (or should not) match could become very tedious. A more compact format using character ranges lets you define a character set to include all of the contiguous characters between a start and stop point. The format used is [start-end].
Common use cases are to search for a specific range of letters in the alphabet, such [a-f] would return matches with any instance of letters between a and f.
Let’s walk through some examples:
‘[a-z]+’ : Represents any lower case characters  and [A-Z] represents upper case.

{% highlight python %}

phrase = 'This is a string! But it has punctutation. How can we remove it?'
reg_exp = re.compile('[a-z]+')
reg_exp.findall(phrase)

output:
['his', 'is', 'a', 'string', 'ut', 'it', 'has', 'punctutation', 
'ow', 'can', 'we', 'remove', 'it']

{% endhighlight %}

‘[a-zA-Z]+’ : Represents lower and upper case characters
{% highlight python %}
phrase = 'This is a string! But it has punctutation. How can we remove it?'
reg_exp = re.compile('[a-zA-Z]+')
reg_exp.findall(phrase)

output:
['This','is', 'a', 'string', 'But', 'it', 'has', 
'punctutation', 'How', 'can', 'we', 'remove', 'it']

{% endhighlight %}

[A-Z][a-z]+ : Represents (1) upper case letter followed by lower case letters.

{% highlight python %}
phrase = 'This is a string! But it has punctutation. How can we remove it?'
reg_exp = re.compile('[A-Z][a-z]+')
reg_exp.findall(phrase)

output:
['This', 'But', 'How']

{% endhighlight %}

**Escape Codes**
You can use special escape codes to find specific types of patterns in your data, such as digits, non-digits,whitespace, and more.
For example: 

{% highlight python %}

Code	Meaning
\d	a digit
\D	a non-digit
\s	whitespace (tab, space, newline, etc.)
\S	non-whitespace
\w	alphanumeric
\W	non-alphanumeric

{% endhighlight %}

\d

{% highlight python %}

phrase = 'This is a string with some numbers 1233 and a symbol #hashtag'
reg_exp = re.compile('\d+')
reg_exp.findall(phrase)

['1233']

{% endhighlight %}

\D

{% highlight python %}

phrase = 'This is a string with some numbers 1233 and a symbol #hashtag'
reg_exp = re.compile('\D+')
reg_exp.findall(phrase)

output:
['This is a string with some numbers ', ' and a symbol #hashtag']

{% endhighlight %}

\s and \S

{% highlight python %}
import re
phrase = 'This is a string with some numbers 1233 and a symbol #hashtag'
reg_exp = re.compile('\s+')
reg_exp.findall(phrase)

[' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ']
{% endhighlight %}

{% highlight python %}

import re
phrase = 'This is a string with some numbers 1233 and a symbol #hashtag'
reg_exp = re.compile('\S+')
reg_exp.findall(phrase)

['This',
 'is',
 'a',
 'string',
 'with',
 'some',
 'numbers',
 '1233',
 'and',
 'a',
 'symbol',
 '#hashtag']

 {% endhighlight %}

 \w and \W

 {% highlight python %}

phrase = 'This is a string with some numbers 1233 and a symbol #hashtag'
reg_exp = re.compile('\w+')
reg_exp.findall(phrase)

['This',
 'is',
 'a',
 'string',
 'with',
 'some',
 'numbers',
 '1233',
 'and',
 'a',
 'symbol',
 'hashtag']

 {% endhighlight %}


{% highlight python %}

phrase = 'This is a string with some numbers 1233 and a symbol #hashtag'
reg_exp = re.compile('\W+')
reg_exp.findall(phrase)

[' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' #']

 {% endhighlight %}
 