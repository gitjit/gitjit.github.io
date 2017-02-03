---
layout: post
title: "Site under Construction"
date: 2017-02-02
tags: [Python,C#]
references: [
   "Google:http://www.google.com",
   "Yahoo:http://www.yahoo.com", 
]

excerpt: "This site is under construction.Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum"
---

Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.

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