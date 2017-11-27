---
layout: post
title: "MapReduce and Cosine Similarity : Movie Recommendation"
description: "A simple movie recommendation program using mapreduce principles"
date:   2017-11-26
tags: [Hadoop,DataScience]
comments: true
references: [
   "MapReduce : http://hadoopgeek.com/mapreduce-movie-recommendation/",
   "Cosine Similarity : https://www.youtube.com/watch?v=C-JauEnlSlM",
   
]
---

In this post we will be writing a map reduce program to recommend movies based on user ratings.  We will be using movie-lens data to generate recommendation. So goal here is to read the movies lens (u.data) and for each movies give recommended movies or similar movies to watch based on their ratings. (For example Is movie '<strong>12 Angry Men</strong>' similar to '<strong>Real Genius</strong>' ?) Let's get started.<!--more-->

The approach we will be using here is called as <strong>Item-based  collaborative filtering</strong>. It involves following steps.
<ol>
 	<li style="text-align: justify;">Find every pair of movies watched by same person</li>
 	<li style="text-align: justify;"> Measure their similarities across all users who watched both</li>
 	<li style="text-align: justify;">Sort by movie, then by similarity strength</li>
</ol>
So the logic here is as following. Let us say that there are 3 users.  (A, B and C).  A watched movie 1 and movie 2 and rated good on those two movies.  User B also watched movie 1 and movie2 and rated good. Now say C watched movie 2, but not watched movie1, then we can recommend movie1 for user C.

<strong>A peek of data
</strong>
<pre class="lang:default decode:true ">u.data
------
userid  movieid rating  timestamp 
196	242	3	881250949
186	302	3	891717742
22	377	1	878887116
244	51	2	880606923
166	346	1	886397596
 
u.item
-------

movieID  Name Release Date  URL
1|Toy Story (1995)|01-Jan-1995||http://us.imdb.com/M/title-exact?|...
2|GoldenEye (1995)|01-Jan-1995||http://us.imdb.com/M/title-exact|..
3|Four Rooms (1995)|01-Jan-1995||http://us.imdb.com/M/title-exact?|...
4|Get Shorty (1995)|01-Jan-1995||http://us.imdb.com/M/title-exact?|..</pre>
<strong>A peek of  final result
</strong>
<pre class="lang:default decode:true">12 Angry Men (1957) [[ Powder (1995)", 0.9500183596757119, 16]
	             ["Game, The (1997)", 0.9500275459249429, 29]
                     ["Ed Wood (1994)", 0.950199438266542, 49]
   	             ["Die Hard: With a Vengeance (1995)", 0.9502072262900199, 48]
	             ["Real Genius (1985)", 0.9512906940485492, 35]
             	     ["Pollyanna (1960)", 0.9514215873345201, 15]
                     ["I.Q. (1994)", 0.9514657003281275, 30]
                     .............................]]
</pre>
So the results above shows movies similar to 12 Angry Men in the data base provided by us.

<strong>Thinking in a MapReduce way</strong>
Now we have to convert the above problem into a map reduce algorithm.  So there will be 3  (mapper and reducer) to reach the final result.    Following are the steps done at each mapper and reducer respectively.  (Note our primary input is u.data and u.item is passed as a parameter to retrieve movie name from movie id)

<span style="color: #808000;">Mapper 1 :  Extract user, (movie,rating)</span>
<span style="color: #808000;"> Input - &gt; u.data</span>
<span style="color: #808000;"> output - &gt;  userID, (movieID, rating)</span>

<span style="color: #808000;">Reducer 1 : Group, user, (movie,rating)</span>
<span style="color: #808000;">Input --&gt; userID, (movieID, rating)</span>
<span style="color: #808000;">output --&gt; userID, [(movieID,rating)(movieID,rating)...]</span>

<span style="color: #808000;">Mapper 2 :  Create movie, rating pairs for each user (combinations)</span>
<span style="color: #808000;">Input - &gt; userID, [(movieID,ratings) (movieID,ratings)...]</span>
<span style="color: #808000;">output -&gt; (movieID,movieID) , (rating, rating)</span>

<span style="color: #808000;">Reducer 2 :  Compute rating based similarity for each movie pair (Cosine Similarity)</span>
<span style="color: #808000;">Input - &gt; (movieID, movieID), ([(rating,rating)(rating,rating)...]</span>
<span style="color: #808000;">Output - &gt; (movieID, movieID), (Similarity Score, Number of users who saw both)</span>

<span style="color: #808000;">Mapper 3 :  Get movie Name and Sort based on similarity Score</span>
<span style="color: #808000;">Reducer 3 : Final result displayed grouped by movie name</span>

<strong>Let's Code
</strong>
<pre class="lang:python decode:true">from mrjob.job import MRJob
from mrjob.step import MRStep
from itertools import combinations
from math import sqrt


class MovieRecommender(MRJob):

    def configure_options(self):
        super(MovieRecommender, self).configure_options()
        self.add_file_option('--items', help='Path to u.item file')

    def load_movie_names(self):
        self.movieNames = {}
        with open('u.item') as f:
            for line in f:
                fields = line.split('|')
                self.movieNames[int(fields[0])] = fields[1]


    def steps(self):
        return [
            MRStep(mapper = self.mapper_extract_user_movie_ratings,
                   reducer = self.reducer_group_user_movie_ratings),
            MRStep(mapper = self.mapper_create_combinations,
                   reducer = self.reducer_calculate_similarity_score),
            MRStep(mapper = self.mapper_sort_similar_movies,
                   mapper_init=self.load_movie_names,
                   reducer = self.reducer_group_similar_movies)
            ]


    def mapper_extract_user_movie_ratings(self, _ , line):
        userID, movieID, rating, timestamp = line.split()
        yield userID, (movieID,float(rating))
    
    def reducer_group_user_movie_ratings(self, userID, movie_ratings):

        ratings = []
        for movie,rating in movie_ratings:
            ratings.append((movie,rating))
        yield userID, ratings

    def mapper_create_combinations(self, userid, movie_ratings_list):
        
       # make combination of all movies_ratings by the user
       # input =  user1, [(m1,2),(m2,3), (m3,4)....]
       # combinations creates a list like this 
       #[((m1,2)(m2,3)), ((m1,2)(m3,4)), ((m2,3)(m3,4)) ... ]
       # make a combination of movies watched by the user, 
       #so we can use same combination on other users in reducer

       for mov_rat1, mov_rat2 in combinations(movie_ratings_list, 2):
           
           movie1 = mov_rat1[0]
           rating1 = mov_rat1[1]

           movie2 = mov_rat2[0]
           rating2 = mov_rat2[1]
           
           yield (movie1, movie2), (rating1, rating2)
           yield (movie2, movie1), (rating2, rating1)

    def calculate_cosine_similarity(self,ratingPairs):
        
        ### Cosing similarity 
        # a . b / sqrt(a^2)) .sqrt((b^2))

        sum_ab = 0
        sum_aa = 0
        sum_bb = 0
        num_pairs = 0
        score = 0

        for a,b in ratingPairs:
            sum_ab += a*b
            sum_aa += a*a
            sum_bb += b*b
            num_pairs += 1

        numerator = sum_ab
        denominator = sqrt(sum_aa) * sqrt(sum_bb)

        if denominator != 0:
            score = numerator / denominator

        return (score, num_pairs)


    def reducer_calculate_similarity_score(self, moviePair, ratingPairs):

        # input is (movies (mov1, mov2) watched by same users
        # and their ratings for that movies
        # (mov1, mov2), [(1,2), (2,3), (3,4) (2, 4).....]
        # calculate cosine similarity , but plotting each ratings as a vector:
        # so if cosine_similarity is &gt; 9.5 means that as per user rating this 
        # movies are similar or recommendable

        score, num_pairs = self.calculate_cosine_similarity(ratingPairs)

        # we are intersted only in good ratings. So if score is close to 1, 
        #then good ratings.
        # also ignore if rating pair count is less than 10

        if (num_pairs &gt; 10 and score &gt; 0.95):
            yield moviePair, (score, num_pairs)
        
    def mapper_sort_similar_movies(self, moviePair, score_rating_count):

        # input is moviepair , score and ratings count
        # output the movie and corresponding paired movie rated
        # also outputs the score generated and total ratings with paired 
        # movie

        try:

            movie1, movie2 = moviePair
            score, total_ratings = score_rating_count

            yield self.movieNames[int(movie1)], self.movieNames[int(movie2)] + 
            '[' + str(score) + ']' + '[' + str(total_ratings) + ']'

        except:
            pass
        

    def reducer_group_similar_movies(self, movie1, similar_movies_score_count):
        
        # just grouping the movies

        m_movies = []

        try:
            for movie in similar_movies_score_count:
                m_movies.append(movie)

            yield movie1, m_movies

        except:
            pass
        

if __name__ == '__main__':
    MovieRecommender.run()
 
</pre>
<strong>Dissecting the code
</strong>We will go through each function and see what its doing.
<pre class="lang:default decode:true">def configure_options(self):
        super(MovieRecommender, self).configure_options()
        self.add_file_option('--items', help='Path to u.item file')</pre>
This method is basically specifying an add file option which enables to specify a file that gets copied to each node in the cluster. In this example we will be passing u.items from which we can get movie names .
<pre class="lang:python decode:true ">def load_movie_names(self):
        self.movieNames = {}
        with open('u.item') as f:
            for line in f:
                fields = line.split('|')
                self.movieNames[int(fields[0])] = fields[1]</pre>
This function creates a dictionary which maps movie id's to movie name. This method gets called in mapper_init which we will discuss below.
<pre class="lang:python decode:true "> def steps(self):
        return [
            MRStep(mapper = self.mapper_extract_user_movie_ratings,
                   reducer = self.reducer_group_user_movie_ratings),
            MRStep(mapper = self.mapper_create_combinations,
                   reducer = self.reducer_calculate_similarity_score),
            MRStep(mapper = self.mapper_sort_similar_movies,
                   mapper_init=self.load_movie_names,
                   reducer = self.reducer_group_similar_movies)
            ]</pre>
This method specifies the mappers , reducers and inits and their order of execution.  Now we will go throug each mappers and reducers and see what they are doing.
<pre class="lang:python decode:true"> def mapper_extract_user_movie_ratings(self, _ , line):
        userID, movieID, rating, timestamp = line.split()
        yield userID, (movieID,float(rating))
    
 def reducer_group_user_movie_ratings(self, userID, movie_ratings):

        ratings = []
        for movie,rating in movie_ratings:
            ratings.append((movie,rating))
        yield userID, ratings</pre>
The above mapper and reducer is pretty straight forward. The mapper extracts userid, (movieid, rating) from the input file (u.data).  The reducer is basically grouping that data for each user in the format userid, [(m1,r1),(m2,r2),(m3,r3)...]
<pre class="lang:python decode:true">def mapper_create_combinations(self, userid, movie_ratings_list):
        
       # make combination of all movies_ratings by the user
       # input =  user1, [(m1,2),(m2,3), (m3,4)....]
       # combinations creates a list like this 
       #[((m1,2)(m2,3)), ((m1,2)(m3,4)), ((m2,3)(m3,4)) ... ]
       # make a combination of movies watched by the user, 
       #so we can use same combination on other users in reducer

       for mov_rat1, mov_rat2 in combinations(movie_ratings_list, 2):
           
           movie1 = mov_rat1[0]
           rating1 = mov_rat1[1]

           movie2 = mov_rat2[0]
           rating2 = mov_rat2[1]
           
           yield (movie1, movie2), (rating1, rating2)
           yield (movie2, movie1), (rating2, rating1)</pre>
The above mapper is basically creating a combination of all movies rated by a particual user and then generating a pair of movies and ratings user watched.
input = user1, [(m1,2),(m2,3), (m3,4)....]
after applying itertools combination on this input we will get following output.
[((m1,2)(m2,3)), ((m1,2)(m3,4)), ((m2,3)(m3,4)) ... ] .From this list we create movie pairs and ratings.
<pre class="lang:python decode:true ">def reducer_calculate_similarity_score(self, moviePair, ratingPairs):

        # input is (movies (mov1, mov2) watched by same users
        # and their ratings for that movies
        # (mov1, mov2), [(1,2), (2,3), (3,4) (2, 4).....]
        # calculate cosine similarity , but plotting each ratings as a vector:
        # so if cosine_similarity is &gt; 9.5 means that as per user rating this 
        # movies are similar or recommendable

        score, num_pairs = self.calculate_cosine_similarity(ratingPairs)

        # we are intersted only in good ratings. So if score is close to 1, 
        #then good ratings.
        # also ignore if rating pair count is less than 10

        if (num_pairs &gt; 10 and score &gt; 0.95):
            yield moviePair, (score, num_pairs)</pre>
The reducer basically groups the movie pair and their ratings and computes the cosine similarity on ratings vector.
<pre class="lang:python decode:true ">def calculate_cosine_similarity(self,ratingPairs):
        
        ### Cosing similarity 
        # a . b / sqrt(a^2)) .sqrt((b^2))

        sum_ab = 0
        sum_aa = 0
        sum_bb = 0
        num_pairs = 0
        score = 0

        for a,b in ratingPairs:
            sum_ab += a*b
            sum_aa += a*a
            sum_bb += b*b
            num_pairs += 1

        numerator = sum_ab
        denominator = sqrt(sum_aa) * sqrt(sum_bb)

        if denominator != 0:
            score = numerator / denominator

        return (score, num_pairs)
</pre>
Here we consider ratings as vectors and calculate the similarity using Cosine Similarity. This gives us a score which if ~1 ensures that those movies based on ratings are pretty similar and can be recommended.
<pre class="lang:python decode:true "> def mapper_sort_similar_movies(self, moviePair, score_rating_count):

        # input is moviepair , score and ratings count
        # output the movie and corresponding paired movie rated
        # also outputs the score generated and total ratings with paired 
        # movie

        try:

            movie1, movie2 = moviePair
            score, total_ratings = score_rating_count

            yield self.movieNames[int(movie1)], self.movieNames[int(movie2)] + 
            '[' + str(score) + ']' + '[' + str(total_ratings) + ']'

        except:
            pass
        

    def reducer_group_similar_movies(self, movie1, similar_movies_score_count):
        
        # just grouping the movies

        m_movies = []

        try:
            for movie in similar_movies_score_count:
                m_movies.append(movie)

            yield movie1, m_movies

        except:
            pass
        
</pre>
The above mapper and reducers are quite straight forward. The mapper basically take first movie and then put the second movie as recommended movie for the first movie in pair. The reducer basically groups that. (For better understanding we are passing scores and number of ratings)

<strong>Execution
</strong>
<pre class="lang:default decode:true">&gt;python MovieRecommender.py --items data\u.item data\u.data &gt; recommend.txt</pre>
<strong>Result (Snap shot)
</strong>
<pre class="lang:default decode:true ">"101 Dalmatians (1996)"	["Anaconda (1997)[0.960001451799][11]", "Angels in the Outfield (1994)[0.951066412551][16]", "Apartment, The (1960)[0.956383090599][13]", "Aristocats, The (1970)[0.958761900926][24]", "Associate, The (1996)[0.961068138791][16]", "Being There (1979)[0.950879042623][21]", "Black Beauty (1994)[0.976345528497][11]", "Bullets Over Broadway (1994)[0.953742320098][14]", "Cape Fear (1962)[0.965237834475][25]", "Casper (1995)[0.954343928284][25]", "Christmas Carol, A (1938)[0.955340806382][17]", "City Hall (1996)[0.950088880101][22]", "Cool Hand Luke (1967)[0.95119036658][24]", "Cool Runnings (1993)[0.958968640301][22]", "Dial M for Murder (1954)[0.95877362392][14]", "Doors, The (1991)[0.959320337593][15]", "Eye for an Eye (1996)[0.958522525608][11]", "Family Thing, A (1996)[0.954668741011][11]", "Fan, The (1996)[0.952366576711][19]", "Father of the Bride (1950)[0.955918126868][14]", "Homeward Bound: The Incredible Journey (1993)[0.951174174976][26]", "Hunt for Red October, The (1990)[0.950775494381][53]", "Indian in the Cupboard, The (1995)[0.973333333333][15]", "Jungle Book, The (1994)[0.967733756994][34]", "Junior (1994)[0.95916356177][19]", "Kalifornia (1993)[0.961318958326][15]", "Kiss the Girls (1997)[0.959191127486][12]", "Madness of King George, The (1994)[0.961297774786][14]", "Malice (1993)[0.972469300022][14]", "Mary Reilly (1996)[0.961379361998][16]", "Michael (1996)[0.952055252505][38]", "Michael Collins (1996)[0.957238206264][21]", "Miller's Crossing (1990)[0.971927392938][11]", "Mortal Kombat (1995)[0.951523657755][12]", "Murder at 1600 (1997)[0.96639967295][33]", "Murder in the First (1995)[0.962492059685][14]", "My Life as a Dog (Mitt liv som hund) (1985)[0.96365930804][15]", "Paper, The (1994)[0.953815175262][12]", "Parent Trap, The (1961)[0.96298010223][27]", "Peacemaker, The (1997)[0.952303833392][12]", "Pete's Dragon (1977)[0.965198954908][17]", "Raging Bull (1980)[0.952290471892][11]", "Secrets &amp; Lies (1996)[0.960549160959][21]", "Shadowlands (1993)[0.950816877414][13]", "She's the One (1996)[0.953171855106][15]", "Strange Days (1995)[0.955636965135][17]", "Substitute, The (1996)[0.963967482826][15]", "Tales From the Crypt Presents: Demon Knight (1995)[0.955014736922][13]", "To Gillian on Her 37th Birthday (1996)[0.957700450455][16]", "Under Siege (1992)[0.954213989666][29]", "Virtuosity (1995)[0.955660434884][13]", "With Honors (1994)[0.957459272341][12]"]
"12 Angry Men (1957)"	["2 Days in the Valley (1996)[0.980690205055][19]", "20,000 Leagues Under the Sea (1954)[0.969262700083][31]", "2001: A Space Odyssey (1968)[0.961979520346][80]", "39 Steps, The (1935)[0.985356481053][33]", "8 1/2 (1963)[0.965109927934][16]", "Absolute Power (1997)[0.965609099171][20]", "Abyss, The (1989)[0.960858720531][39]", "Adventures of Priscilla, Queen of the Desert, The (1994)[0.965537900456][36]", "Adventures of Robin Hood, The (1938)[0.980349115821][36]", "Affair to Remember, An (1957)[0.986687449779][14]", "African Queen, The (1951)[0.979551527392][59]", "Age of Innocence, The (1993)[0.971844896213][24]", "Air Force One (1997)[0.952529271901][44]", "Akira (1988)[0.960859221641][18]", "Aladdin (1992)[0.955029304991][59]", "Alien (1979)[0.974849484133][78]", "Aliens (1986)[0.963869612761][74]", "All About Eve (1950)[0.980280105642][30]", "Amadeus (1984)[0.977913534905][88]", "American in Paris, An (1951)[0.987774556665][24]", "American Werewolf in London, An (1981)[0.958420884414][30]", "Amistad (1997)[0.955297288929][14]", "Annie Hall (1977)[0.956971556552][65]", "Antonia's Line (1995)[0.975082150877][12]", "Apartment, The (1960)[0.96347130036][35]", "Apocalypse Now (1979)[0.9664281618][77]", "Apollo 13 (1995)[0.967469639061][78]", "Apt Pupil (1998)[0.962988181998][19]", "Army of Darkness (1993)[0.959863561746][33]", "Around the World in 80 Days (1956)[0.95649560904][31]", "Arsenic and Old Lace (1944)[0.978000540456][47]", "As Good As It Gets (1997)[0.974938596654][16]",</pre>
<strong>References:</strong>

https://www.youtube.com/watch?v=C-JauEnlSlM

https://www.youtube.com/watch?v=-gz1qdsM0tk

http://www.coranac.com/tonc/text/matrix.htm
