---
layout: post
title:  "Python : CSV data analysis"
description: "Generating a dictionary from CSV"
date:   2011-04-07
tags: [Python]
comments: true
references: [
  "Standard library : https://docs.python.org/3/library/index.html",
   "CSV Reader : https://docs.python.org/3/library/csv.html",
   "Zip Function : https://docs.python.org/3/library/functions.html?highlight=zip#zip",
   "Iter Tools : https://docs.python.org/3/library/itertools.html",
]

excerpt: "In this post we will be doing some simple data analysis from CSV files using core-python API's. We will not be
using any data libraries. This will give us a chance to excercise some of the core python features like sorting, groupby and
comprehensions. 
"
---  

In this post we will be doing some simple data analysis from CSV files using core-python API's. We will not be
using any data libraries. This will give us a chance to excercise some of the core python features like sorting, groupby and
comprehensions. This post is an extension to my previous post[ generating dictionary from CSV](http://pepgin.com/csv-reader/).  

What is the data we are analysing ?   

Data we are dealing with are bunch of small csv files, which contains some stock portfolios as shown below.  It contains
`name, date of purchase, number of shares purchased and price` at the time of purchase. A sample file looks like this.

<pre class='line-numbers'>
<code class='language-bash'>
**Name,Date,Shares,Price**
HPQ,7/11/2007,100,32.2
IBM,7/12/2007,50,91.9
GE,7/13/2007,150,83.44
CAT,7/14/2007,200,51.23
MSFT,7/15/2007,95,40.37
HPE,7/16/2007,50,65.1
AFL,7/17/2007,100,70.44
</code>
</pre>

<img src='/images/2017-04-21-10-48-26.png' class='img-responsive'>

There will be multiple files residing in a folder ('stocks') in the same format as shown above, so we have to read all files and 
process it. As a first step, let us read all csv files and generate a list of dictionary from it. We will be using the `csvreader`
module we created before.  


<pre class='line-numbers'>
<code class='language-python'>
def get_portfolio_as_dict(path):
    pattern = '*.csv'
    files = glob.glob(os.path.join(path,pattern))
    portfolios = []
    for file in files:
        for row in csvreader.read_csv(file,[str, str, int, float]):
            portfolios.append(row)
    return portfolios


</code>
</pre>

`get_portfolio_as_dict(path)` expects a folder location where all 'csv' files resides. We are using `glob` module
which returns a list of files that matches the pattern as shown in `line:3`. After that we read's each csv
file and convert each row in that file to a dictionary and appends it to a list and returns the record. A snap shot
of output looks like this .  

<pre class='line-numbers'>
<code class='language-python'>
records =  get_portfolio_as_dict('stocks')
pprint.pprint(records)
</code>
</pre>

<pre class='line-numbers'>
<code class='language-json'>

# output

[{'Date': '7/11/2007', 'Name': 'HPQ', 'Price': 32.2, 'Shares': 100},
 {'Date': '7/12/2007', 'Name': 'IBM', 'Price': 91.9, 'Shares': 50},
 {'Date': '7/13/2007', 'Name': 'GE', 'Price': 83.44, 'Shares': 150},
 {'Date': '7/14/2007', 'Name': 'CAT', 'Price': 51.23, 'Shares': 200},
 {'Date': '7/15/2007', 'Name': 'MSFT', 'Price': 40.37, 'Shares': 95},
 {'Date': '7/16/2007', 'Name': 'HPE', 'Price': 65.1, 'Shares': 50},
 {'Date': '7/17/2007', 'Name': 'AFL', 'Price': 70.44, 'Shares': 100},
 {'Date': '7/11/2007', 'Name': 'GOOGL', 'Price': 55.2, 'Shares': 100},
 {'Date': '7/12/2007', 'Name': 'IBM', 'Price': 91.9, 'Shares': 50},
 {'Date': '7/13/2007', 'Name': 'GE', 'Price': 83.44, 'Shares': 150},
 {'Date': '7/14/2007', 'Name': 'CAT', 'Price': 51.23, 'Shares': 200},
 {'Date': '7/15/2007', 'Name': 'MSFT', 'Price': 40.37, 'Shares': 95},
 {'Date': '7/16/2007', 'Name': 'HPE', 'Price': 65.1, 'Shares': 50},
 {'Date': '7/17/2007', 'Name': 'AFL', 'Price': 70.44, 'Shares': 100},
 {'Date': '7/11/2007', 'Name': 'GOOGL', 'Price': 55.2, 'Shares': 100},
 {'Date': '7/12/2007', 'Name': 'IBM', 'Price': 91.9, 'Shares': 50},
 {'Date': '7/13/2007', 'Name': 'GE', 'Price': 83.44, 'Shares': 150},
 {'Date': '7/14/2007', 'Name': 'TCS', 'Price': 51.23, 'Shares': 200},
 {'Date': '7/15/2007', 'Name': 'MSFT', 'Price': 43.37, 'Shares': 95},
 {'Date': '7/16/2007', 'Name': 'INFY', 'Price': 62.1, 'Shares': 50}]

</code>
</pre>

##### Get Holding Names  

Now let us grab all unique holding names from the portfolios. We are using
`set comprehension` to collect unique names.

<pre class='line-numbers'>
<code class='language-'>
port_names = get_portfolio_names('stocks')
print(port_names)
</code>
</pre>

<pre class='line-numbers'>
<code class='language-python'>
def get_portfolio_names(path):
    portfolios = get_portfolio_as_dict(path)
    names = { p['Name'] for p in portfolios}
    return names
</code>
</pre>

output..

`{'INFY', 'HPE', 'CAT', 'AFL', 'GE', 'TCS', 'GOOGL', 'HPQ', 'IBM', 'MSFT'}`  

#### Total Value of holdings  

<pre class='line-numbers'>
<code class='language-python'>
def get_total_value(path):
    portfolios = get_portfolio_as_dict(path)
    total = sum([p['Shares'] * p['Price'] for p in portfolios])
    return total
</code>
</pre>

<pre class='line-numbers'>
<code class='language-python'>
total_value = get_total_value('stocks')
print('Total value is {}'.format(total_value))
</code>
</pre>

output..

`Total value is 131824.44999999998`

#### Current value of each holding  
In this case we are using `request` module to grab latest values from yahoo url.

<pre class='line-numbers'>
<code class='language-python'>
def get_current_value(path):
    names = get_portfolio_names(path)
    yahoo_url = 'http://finance.yahoo.com/d/quotes.csv?s={}&f=l1'.format(names)
    cur_values = requests.get(yahoo_url).text
    cur_values = cur_values.split()
    name_value = zip(names, cur_values)
    for name,value in name_value:
        print(name,value)

</code>
</pre>

output..

<pre class='line-numbers'>
<code class='language-bash'>
GE 29.565
CAT 94.46
HPE 18.10
GOOGL 859.66
HPQ 18.38
MSFT 66.315
INFY 14.385
AFL 74.23
TCS 4.15
IBM 160.81
</code>
</pre>

#### Group By  
Since our data is spread across multiple files let us group by Name. We are using 
`itertools.groupby` in which you can specify a key to be used for grouping using
a `lambda`.

<pre class='line-numbers'>
<code class='language-python'>
def print_sorted_portfolios(path):
    portfolios = get_portfolio_as_dict(path)
    portfolios.sort(key=lambda x: x['Name'])
    for name, items in itertools.groupby(portfolios, lambda x:x['Name']):
        print(name)
        for item in items:
            print('   ', item)
</code>
</pre>
 
 output..

 <pre class='line-numbers'>
 <code class='language-json'>
AFL
    {'Shares': 100, 'Date': '7/17/2007', 'Price': 70.44, 'Name': 'AFL'}
    {'Shares': 100, 'Date': '7/17/2008', 'Price': 70.44, 'Name': 'AFL'}
CAT
    {'Shares': 200, 'Date': '7/14/2007', 'Price': 51.23, 'Name': 'CAT'}
    {'Shares': 200, 'Date': '7/14/2008', 'Price': 51.23, 'Name': 'CAT'}
GE
    {'Shares': 150, 'Date': '7/13/2007', 'Price': 83.44, 'Name': 'GE'}
    {'Shares': 150, 'Date': '7/13/2008', 'Price': 83.44, 'Name': 'GE'}
    {'Shares': 150, 'Date': '7/13/2009', 'Price': 83.44, 'Name': 'GE'}
GOOGL
    {'Shares': 100, 'Date': '7/11/2008', 'Price': 55.2, 'Name': 'GOOGL'}
    {'Shares': 100, 'Date': '7/11/2009', 'Price': 55.2, 'Name': 'GOOGL'}
HPE
    {'Shares': 50, 'Date': '7/16/2007', 'Price': 65.1, 'Name': 'HPE'}
    {'Shares': 50, 'Date': '7/16/2008', 'Price': 65.1, 'Name': 'HPE'}
HPQ
    {'Shares': 100, 'Date': '7/11/2007', 'Price': 32.2, 'Name': 'HPQ'}
IBM
    {'Shares': 50, 'Date': '7/12/2007', 'Price': 91.9, 'Name': 'IBM'}
    {'Shares': 50, 'Date': '7/12/2008', 'Price': 91.9, 'Name': 'IBM'}
    {'Shares': 50, 'Date': '7/12/2009', 'Price': 91.9, 'Name': 'IBM'}
INFY
    {'Shares': 50, 'Date': '7/16/2009', 'Price': 62.1, 'Name': 'INFY'}
MSFT
    {'Shares': 95, 'Date': '7/15/2007', 'Price': 40.37, 'Name': 'MSFT'}
    {'Shares': 95, 'Date': '7/15/2008', 'Price': 40.37, 'Name': 'MSFT'}
    {'Shares': 95, 'Date': '7/15/2009', 'Price': 43.37, 'Name': 'MSFT'}
TCS
    {'Shares': 200, 'Date': '7/14/2009', 'Price': 51.23, 'Name': 'TCS'}

 </code>
 </pre>  

#### Value of each holding
 
 Now we grouped by name, we can easily calculate total value of each holding.  

 <pre class='line-numbers'>
 <code class='language-python'>
 def get_portfolio_values(path):
    portfolios = get_portfolio_as_dict(path)
    portfolios.sort(key=lambda x: x['Name'])
    holding_values = {}
    for name, items in itertools.groupby(portfolios, lambda x: x['Name']):
        total_value = 0
        for item in items:
            total_value += item['Shares'] * item['Price']
        holding_values[name] = total_value
    return holding_values
 </code>
 </pre>

output..

<pre class='line-numbers'>
<code class='language-bash'>
{'CAT': 20492.0, 'GOOGL': 11040.0, 'IBM': 13785.0, 
'MSFT': 11790.449999999999, 'INFY': 3105.0, 
'GE': 37548.0, 'HPQ': 3220.0000000000005, 'AFL': 14088.0, 
'HPE': 6509.999999999999, 'TCS': 10246.0}
</code>
</pre>  

#### Top Valued  

Now we got a dictionary of each portfolio and its values. Now to calculate the top one we
have to sort the dictionary. Python don't have a straight forward way to sort a dictionary,
so we are using `sorted` method in which we are passing the key to be sorted as the Price.

<pre class='line-numbers'>
<code class='language-python'>
def get_top_portfolio(path):
    portfolio_values = get_portfolio_values(path)
    sorted_by_value = sorted(portfolio_values.items(), key=lambda x:x[1])
    return sorted_by_value[-1]
</code>
</pre>
 
 output..  
 
` ('GE', 37548.0)`  

_Coding is fun enjoy..._  
