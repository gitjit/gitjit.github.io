---
layout: post
title: "MapReduce : Matrix Multiplication"
description: "Understanding mapreduce through matrix multiplication"
date:   2013-07-21
tags: [Hadoop,DataEngineer]
comments: false
references: [
   "MapReduce : http://hadoopgeek.com/mapreduce-movie-recommendation/",
   
]
---  

In one of my earlier post,I discussed about matrix multiplcation using SQL. Here I will explain how to do <a href="http://en.wikipedia.org/wiki/Matrix_multiplication" target="_blank">matrix multiplication </a>using<a href="http://hadoopgeek.com/?p=198" target="_blank"> MapReduce</a>. This will be efficient when we are dealing with huge matrices with thousands of rows and columns. Here we are expecting the input dataset representing matrix as a relation in database.

<strong><span style="color: #008000;">Matrix in Database</span>
</strong>We can represent a <a href="http://en.wikipedia.org/wiki/Matrix_(mathematics)" target="_blank">matrix </a>as a relation (table) in RDBMS  where each cell in the matrix can be represented as a record (i,j,value).  As an example let us consider the following matrix and its representation.
<span style="color: #3366ff;">0 2 -1</span>
<span style="color: #3366ff;">1 0 0</span>
<span style="color: #3366ff;">0 0 -3</span>
<span style="color: #3366ff;">0 0 0</span>
[table]
row#,col#,value
0,1,2
0,2,-1
1,0,1
2,2,-3
[/table]

It is important to understand that this relation is a very <strong>inefficient</strong> relation if the matrix is dense.  Let us say we have 5 Rows and 6 Columns , then we need to store only 30 values. But if you consider above relation we are storing  30 rowid, 30 col_id and 30 values in other sense we are tripling the data. So a natural question arises why we need to store in this format ?  In practice most of the matrices are<a href="http://en.wikipedia.org/wiki/Sparse_matrix"><span style="color: #000080;"> sparse matrices</span> </a>. In sparse matrices not all cells used to have any values , so we don't have to store those cells in DB. So this  turns out to be very efficient in storing such matrices. For this example I am using the same approach and store the matrix in the same format as mentioned above.
<span style="color: #008000;"><strong>Input Dataset</strong></span>
For ease of explanation I am considering  here multiplcation of  two<a href="http://en.wikipedia.org/wiki/Square_matrix" target="_blank"> square matrices</a> as shown below. Try this online<a href="http://www.mathsisfun.com/algebra/matrix-calculator.html" target="_blank"> matrix calculator</a>, if you want to try matrix computations.  

<img src='/images/2017-11-26-21-18-51.png' class='img-responsive'>

The input data set containing these matrices are represented as follows.

[code language="sql"]
a, 0, 0, 63
a, 0, 1, 45
a, 0, 2, 93
a, 0, 3, 32
a, 0, 4, 49
a, 1, 0, 33
a, 1, 3, 26
a, 1, 4, 95
a, 2, 0, 25
a, 2, 1, 11
a, 2, 3, 60
a, 2, 4, 89
a, 3, 0, 24
a, 3, 1, 79
a, 3, 2, 24
a, 3, 3, 47
a, 3, 4, 18
a, 4, 0, 7
a, 4, 1, 98
a, 4, 2, 96
a, 4, 3, 27
b, 0, 0, 63
b, 0, 1, 18
b, 0, 2, 89
b, 0, 3, 28
b, 0, 4, 39
b, 1, 0, 59
b, 1, 1, 76
b, 1, 2, 34
b, 1, 3, 12
b, 1, 4, 6
b, 2, 0, 30
b, 2, 1, 52
b, 2, 2, 49
b, 2, 3, 3
b, 2, 4, 95
b, 3, 0, 77
b, 3, 1, 75
b, 3, 2, 85
b, 4, 1, 46
b, 4, 2, 33
b, 4, 3, 69
b, 4, 4, 88
[/code]

<span style="color: #008000;"><strong>MapReduce : Logic
</strong><span style="color: #000000;">So logic here is to send the calculation part of each output cell of the result matrix to a reducer.  So in matrix multiplication the first cell of <strong>output</strong>  (0,0) has multiplication and summation of elements from row 0 of the matrix A and elements from col 0 of matrix B.  To do the computation of value  in the output cell (0,0) of resultant matrix  in a seperate reducer we need to use (0,0) as output key of mapphase and value should have array of values from row 0 of matrix A and column 0 of matrix B.  Hopefully this picture will explain the point. </span></span>

<span style="color: #008000;"><span style="color: #000000;"> 
<img src='/images/2017-11-26-21-20-23.png' class='img-responsive'>

So in this algorithm output from map phase should be having a <key,value> , where  key represents the output cell location (0,0) , (0,1) etc.. and value will be list of all values required for reducer to do computation. Let us take an example for calculatiing value at output cell (00). Here we need to collect values from row 0 of matrix A and col 0 of matrix B in the map phase and pass (0,0) as key. So a single reducer can do the calculation.

Implementation
In this implementation for ease of understanding I have hardcoded the dimension of matrix as (5 * 5). 
Map
In the map function each input from the dataset is organized to produce a key value pair such that reducer can do the entire computation of the corresponding output cell. The source code is given below.

[code language=”java”]
public class MatrixMapper extends
Mapper<LongWritable, Text, Text, Text>
{
@Override
protected void map
(LongWritable key, Text value, Context context)
throws IOException, InterruptedException
{
// input format is ["a", 0, 0, 63]
String[] csv = value.toString().split(",");
String matrix = csv[0].trim();
int row = Integer.parseInt(csv[1].trim());
int col = Integer.parseInt(csv[2].trim());
if(matrix.contains("a"))
{
for (int i=0; i < lMax; i++)
{
String akey = Integer.toString(row) + "," +
Integer.toString(i);
context.write(new Text(akey), value);
}
}
if(matrix.contains("b"))
{
for (int i=0; i < iMax; i++)
{
String akey = Integer.toString(i) + "," +
Integer.toString(col);
context.write(new Text(akey), value);
}
}
}
}
[/code]

 Reducer
Input to the reducer is the key that corresponds to the output cell of resultant matrix and values required to do computation of that cell.  The source code of reduce function is given below.

[code language=”java”]
public class MatrixReducer extends
Reducer<Text, Text, Text, IntWritable> {

@Override
protected void reduce
(Text key, Iterable<Text> values, Context context)
throws IOException, InterruptedException {

int[] a = new int[5];
int[] b = new int[5];
// b, 2, 0, 30
for (Text value : values) {
System.out.println(value);
String cell[] = value.toString().split(",");
if (cell[0].contains("a")) // take rows here
{
int col = Integer.parseInt(cell[2].trim());
a[col] = Integer.parseInt(cell[3].trim());
}
else if (cell[0].contains("b")) // take col here
{
int row = Integer.parseInt(cell[1].trim());
b[row] = Integer.parseInt(cell[3].trim());
}
}
int total = 0;
for (int i = 0; i < 5; i++) {
int val = a[i] * b[i];
total += val;
}
context.write(key, new IntWritable(total));
}
}
[/code]

Output
The above MR job will generate output as shown below.

[code language=”sql”]
0,0 11878
0,1 14044
0,2 16031
0,3 5964
0,4 15874
1,0 4081
1,1 6914
1,2 8282
1,3 7479
1,4 9647
2,0 6844
2,1 9880
2,2 10636
2,3 6973
2,4 8873
3,0 10512
3,1 12037
3,2 10587
3,3 2934
3,4 5274
4,0 11182
4,1 14591
4,2 10954
4,3 1660
4,4 9981
[/code]
