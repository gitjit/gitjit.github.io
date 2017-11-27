---
layout: post
title: "MapReduce : Passing additional file"
description: "Passing additional file in MapReduce"
date:   2013-10-21
tags: [Hadoop,DataScience]
comments: false
references: [
   "MapReduce : http://hadoopgeek.com/mapreduce-movie-recommendation/",
   
]
---

In an earlier post, we discussed passing additional parameters to MapReduce Job. But there are cases in which we will have to pass some additional files during MapReduce. But since MapReduce runs in multiple nodes, we need to ensure that this additional file that mapper/reducer refers is in that particular node in which its running.  In this post we will disucss how to handle this. Let us say we need to find most popular movie from movie-lens database. If you download movie-lens data, there are 2 files in which we are interested in .  (u.data and u.item). The format of file is as shown here..<!--more-->  

<pre class="lang:default decode:true">u.data
userid  movieid rating  timestamp 
196	242	3	881250949
186	302	3	891717742
22	377	1	878887116
244	51	2	880606923
166	346	1	886397596

u.item
movieID  Name Release Date  URL
1|Toy Story (1995)|01-Jan-1995||http://us.imdb.com/M/title-exact?|...
2|GoldenEye (1995)|01-Jan-1995||http://us.imdb.com/M/title-exact|..
3|Four Rooms (1995)|01-Jan-1995||http://us.imdb.com/M/title-exact?|...
4|Get Shorty (1995)|01-Jan-1995||http://us.imdb.com/M/title-exact?|..
</pre>
<strong>Implementation:
</strong>
<pre class="lang:python decode:true">from mrjob.job import MRJob
from mrjob.step import MRStep

'''
    Goal is to calculate most popular movie from the movie lens data 
    Algorithm :  Most watched movie is most popular movie
    Also pass u.item file along with job, so that it will be available in    each nodes, so we can map movieID to a moviename
'''

class MRMostPopularMovieName(MRJob):


    def configure_options(self):
        super(MRMostPopularMovieName, self).configure_options()
        self.add_file_option('--items', help='path to u.item') 



    def steps(self):
        return [
                     MRStep(mapper = self.mapper_get_count,
                            reducer_init = self.reducer_init,
                            reducer = self.reducer_sum_count),

                     MRStep( reducer = self.reducer_max_count)

               ]


    def mapper_get_count(self, _ , line):
        userId, movieID, rating, timeStamp = line.split('\t')
        yield movieID, 1


    def reducer_init(self):
        self.movie_names = {}

        with open("u.item") as f:
            for line in f:
                fields = line.split('|')
                self.movie_names[fields[0]] = fields[1]

    
    def reducer_sum_count(self, movieID, values):
        yield None , (sum(values), self.movie_names[movieID])
        

    def reducer_max_count(self, _ , views):
        yield max(views)


if __name__ == '__main__':
    MRMostPopularMovieName.run()</pre>
<pre class="lang:default decode:true">&gt;&gt; python most_popular_movie_name.py --items data\ml-100k\u.item data\ml-100k\u.data</pre>
<strong>Configure Options
</strong>In the function configure_option  we have used add file option to specify that a file will be passed along with this MapReduce Job and should be copied to each node.  We have also mentioned that the file will be identified by --items in the command line arg along with the actual data file.

<strong>MRJob Step
</strong>We have also specified some additional steps including a reducer_init which will get called before the reducer in that step gets called. In reducer_init, we create a dictionary of movie names from u.item file which will be present in the same node where mapper/reducer is running.

Another important thing to note in this example is that reducer is passing 'None' as key and a tuple of  sum(views) and movie_name  <strong>(sum(values), self.movie_names[movieID])</strong> as value. This  will help us to calculate the max(views) in the next reducer, which will produce a single result. 