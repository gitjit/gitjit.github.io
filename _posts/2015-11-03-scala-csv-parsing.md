---
layout: post
title:  "Scala : CSV Parsing"
date:   2015-11-03 8:50:22
categories: Scala
author : Jithesh Chandrasekharan
image: fc-climate.png
comments: true
---

In this post we will doing a file I/O in Scala.We will be reading a csv file which contains climate records of Fort Collins (CO) downloaded from <a target="_blank" href = "http://cdiac.ornl.gov/epubs/ndp/ushcn/ushcn_map_interface.html">CDIAC</a> . The dataset is downloaded as a csv file containing daily climate recording starting 1893 to 2014. We will calculate lowest temperature, highest temperature and highest snow fall recorded in the region during this period.

The picture above shows first few rows in the data set.

{% highlight scala %}

object scalasamples {


new java.io.File(".").getAbsolutePath() // gives path looking for files

val src = io.Source.fromFile("/Users/jit/Downloads/CO053005_5842.csv")

                                                  
val lines = src.getLines()   

lines.next() //ignore two lines                              
                                                  
lines.next()                                     
                                                  
case class TempData(day:Int, month:Int, year:Int, percp:Double,
			 snowFall:Double, tMax:Int, tMin:Int)
                                                
def parseData(line:String)=
{
   val p = line.split(",").map(_.trim).
	 map((x) => if(x == ".") "0" else x) // replace space and '.'

   TempData(p(0).toInt, p(2).toInt, p(4).toInt,p(5).
		toDouble,p(6).toDouble, p(7).toInt,p(8).toInt)	
	
}                                                 

val data = lines.map(parseData).toArray     

val max = data.map(_.tMax).max      //> max  : Int = 103

val min = data.map(_.tMin).min      //> min  : Int = -41

val max_snow = data.map(_.snowFall).max //> max_snow  : Double = 21.1

val year = data.map(_.year).min      //> year  : Int = 1893

src.close()

}
{% endhighlight %}

We use Scala.io.Source.fromFile to read the csv and use getLines to read each line as a String. Then we defined a case Class TempData with required fields to store the cleaned results. 





