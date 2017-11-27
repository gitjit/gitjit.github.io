---
layout: post
title: "MapReduce : Chaining"
description: "Mapreduce chaining where output of reducer will be input of another mapper."
date:   2013-10-20
tags: [Hadoop]
comments: false
references: [
   "MapReduce : http://hadoopgeek.com/mapreduce-movie-recommendation/",
   
]
---  

In this post I will be explaining how  to add chaining in your map reduce job. That is output of  reducer will be chained as an input to another mapper in same job.  As an example to explain this I will be improving our regular word count program. In word count program we will get the output as a word and how many occurances of that word in input book.  But if we could sort that output based on count, we can easily predict what this books is all about.  So let's get started.<!--more-->

I am using python's MRJob package for writing the map reduce job. If you have a python environment already installed in your machine, you just need to use this command to install MRJob package.
<pre class="lang:default decode:true ">&gt;&gt; pip install mrjob</pre>
<strong>Implementation:
</strong>
<pre class="lang:python decode:true">from mrjob.job import MRJob
from mrjob.step import MRStep
import re


#wc_regex = re.compile(r"['\w']+")
wc_regex = re.compile(r"\w+")

class MRWord_freq_count(MRJob):

    def mapper_get_words(self, key, line):

        words = wc_regex.findall(line)
        for word in words:
            yield word.lower(), 1

    def reducer_count_words(self, word, values):
        ''' sum up count of each words'''
        yield word, sum(values)

    def mapper_count_keys(self, word, count):
        yield '%04d'%int(count), word

    def reducer_output_words(self, count, words):
        for word in words:
            yield count, word 

    def steps(self):
        return  [
            MRStep(mapper = self.mapper_get_words, 
                   reducer = self.reducer_count_words),
            MRStep(mapper = self.mapper_count_keys, 
                   reducer = self.reducer_output_words)
            ]

if __name__ == '__main__':
    MRWord_freq_count.run()</pre>
<strong>What's happening?
</strong>A job is defined by a class that inherits from <a class="reference internal" title="mrjob.job.MRJob" href="http://mrjob.readthedocs.org/en/latest/job.html#mrjob.job.MRJob"><code class="xref py py-class docutils literal"><span class="pre">MRJob</span></code></a>. This class contains methods that define the <a class="reference internal" href="http://mrjob.readthedocs.org/en/latest/glossary.html#term-step"><span class="xref std std-term">steps</span></a>of your job.
A “step” consists of a mapper, a combiner, and a reducer. All of those are optional, though you must have at least one. So you could have a step that’s just a mapper, or just a combiner and a reducer.
When you only have one step, all you have to do is write methods called <a class="reference internal" title="mrjob.job.MRJob.mapper" href="http://mrjob.readthedocs.org/en/latest/job.html#mrjob.job.MRJob.mapper"><code class="xref py py-meth docutils literal"><span class="pre">mapper()</span></code></a>, <a class="reference internal" title="mrjob.job.MRJob.combiner" href="http://mrjob.readthedocs.org/en/latest/job.html#mrjob.job.MRJob.combiner"><code class="xref py py-meth docutils literal"><span class="pre">combiner()</span></code></a>, and<a class="reference internal" title="mrjob.job.MRJob.reducer" href="http://mrjob.readthedocs.org/en/latest/job.html#mrjob.job.MRJob.reducer"><code class="xref py py-meth docutils literal"><span class="pre">reducer()</span></code></a>.
The <code class="xref py py-func docutils literal"><span class="pre">mapper()</span></code> method takes a key and a value as args (in this case, the key is ignored and a single line of text input is the value) and yields as many key-value pairs as it likes. The <a class="reference external" title="(in Python v2.7)" href="http://docs.python.org/2/library/functions.html#reduce"><code class="xref py py-func docutils literal"><span class="pre">reduce()</span></code></a> method takes a key and an iterator of values and also yields as many key-value pairs as it likes. (In this case, it sums the values for each key, which represent the numbers of characters, words, and lines in the input.)

In this particular example, I have added a new mapper, which takes the output from reducer and generate a key,value pair such that it interchanges the key/value  it recieves such that the count is converted to string (for better sorting) and make it the key and word as the value.
So here we are using default sorting mechanism of map reduce frame work, so the next reducer will get input sorted based on frequency rather than word. So from the reducer output which is sorted based on frequency, its easy to understand which is the most frequently used  meaningful word in this book. This word gives us more insight on what this book is about.
<pre class="lang:default decode:true">&gt;&gt; python wc_freq_counter.py book.txt &gt;&gt; out.txt</pre>
It is important to not that if your job is targetting multiple nodes , then sorting will happen only for the results in that particular node.