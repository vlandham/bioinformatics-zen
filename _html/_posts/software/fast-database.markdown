---
  category: software
  layout: default
---
Bioinformatics usually a involves shuffling data into the right format for plotting or statistical tests. I prefer to [use a database to store and format data][database] as I think this make projects easier to maintain compared with using lots of different scripts. I find a dynamic language like Ruby and the corresponding libraries for database manipulation like [ActiveRecord][ar] makes using a database in research relatively simple.

Using a database however stops being simple when you have to deal with very large amounts of data. Here I'm going to write about my experience of analysing gigabytes of data with millions of data points. The ideas I'm outlining here may not be the best way to approach dealing with large data sets I have use them all. I've the list from what I think is the least to most technical, or in other words the most pragmatic approaches are first.

## The simple things

### 1. Use existing data

Not really a tip, but if you can get the data you need from someone else then you don't even need to run any analysis. This may seem obvious but I know from experience how how strong the urge is to start coding in a new research project. Nevertheless an effective way to save time and effort is to rely on existing data rather than jumping in head first with your own code and analysis.

### 2. Use a bigger computer

Using a faster computer might seem like a lazy option compared with optimising your code, but if the analysis works on your computer it should work the same, but faster, on a better computer. Using a faster computer is probably one of the few things I tried which which didn't involved modifying my code and therefore shouldn't have introduced any bugs. I used unit tests to make sure the code still worked as expected though.

### 3. Add database indices

Since I'm using a database making sure it runs as fast as possible is another cheap way to improve performance. Properly indexed database columns reduce running times when searching or joining tables since database rows are looked up much faster. Database indices is also relatively easy, just specify which columns need to be indexed either using SQL or the ORM.

### 4. Use a faster language interpreter

Most of the time the standard version of ruby sufficient for running my code. In last two years though different but faster versions of Ruby have been created such as [REE][ree], [JRuby][jruby] and [Ruby 1.9][ruby19]. Therefore when I was working with a very large amount of data and having problems with running times I though it was worth a try using a new faster version of Ruby. I tried running my code using Ruby 1.9 and it did improve performance. One caveat though was that I had to make my code compatible with the new version [specifically for the CSV library][csv]. These code changes were still relatively easy to implement though and using an improved language interpreter did return a noticeable benefit.

## Delete stuff

After the above three points I generally had to start digging around in the code - which is bad because changing working code creates bugs. A good way to optimise code, without introducing too many new problems, is just to delete it entirely.

### 5. Delete unnecessary data and analysis

I find that I often generate variables which I think might be useful at some future time. As you might expect just deleting the code that produces these variables saves time and more often than not the variable wasn't never required anyway.

### 6. Remove database table joins

I'm using a database because usually I want to compare two or more sets of data and therefore I need to format them in a way that makes them comparable. Once formatted I join each variable in the database and then print the join as a csv file.

The problem with joining a large number of database records, even with indices, is that it can take a very long time. The amount of time required also increases the more the data is denormalised. To try and remedy this I found that I could drop the smaller of two variables and instead do the data join further into my workflow.

For example I had two sets of data, the first contained millions of entries where each one corresponded to a protein residue. The second data contained around only 100 entries where each correspond to one of twenty amino acids. Merging these two variables in my database required millions of joins and took a long time. Instead I joined my amino acid data to protein data after I had calculated the mean of each protein residue. This reduced the number of joins from a million down to around 100. I did the join relatively easily using the [merge function in R][merge].

## Code optimisation

When I was encountering performance problems I left optimising code as a last resort. There were three reasons for this, the first is that [premature optimisation is the root of all evil][premature]. The second reason reason is that the enemy of good-enough code is perfect code - when I start optimising code I tend keep going more than is necessary. Code doesn't need to be as fast as possible though, just fast enough to get the desired results. The third point is that optimising code, means changing code, which introduces bugs and so the more the code is optimised the more chance of bugs. Unfortunately code optimisation for performance is usually a necessity, but remembering it's ultimately better to produce the right results slowly than the wrong results quickly.

### 7A. Batch load database query results

One easy way I reducing running times was by batch loading the database rows into memory rather than trying to process a very large table or query at one. Pulling all the database records into memory means that most the time is spent loading the data into memory rather than actually dealing with it. By instead pulling subsets of records into memory at a time, each subset is processed before the next set or rows is retrieved meaning less memory used up. A example of this in Ruby is [the ActiveRecord method find_in_batches][batches]. 

### 7B. Association loading

Association loading means that when a row of Table A is retrieved from the database that all the rows associated with it in Table B are also retrieved at the same time. This will usually mean that the database is only queried twice, once to find the records from Table A and once to find the records from Table B. The alternative option is to use a loop to retrieve each row from Table B when it's required but this will mean as many database queries as there are rows, and more queries means more running time.

### 7C. Database querying in loops

I found that large loops which query the database were often the majority of my software's running time. I improves this by instead moving the database calls, up or out of the loops as much as possible and caching rows in memory before I needed them where ever possible. This meant the looping code was looking things up in memory rather then querying the database each time. A similar approach aim at avoiding object creation in loops also seemed improve performance. Combining this approach with the one below was what saved the most amount of time for my project.

### 7D. Use raw SQL

Object relational management (ORM) libraries like ActiveRecord allow the database to be manipulated using object orientated programming which makes using a database easier. Using an ORM does however add a performance penalty because it is an extra layer of code on top of the database. When I was doing millions of database updates I found that skipping using the ORM and directly using raw SQL contributed to a large saving of processing time. The advantages of this technique are neatly outlined [by Ilya Grigorik][ilya].

## That's it.

Quite a long post I know. I'm also sure that if you've done any work with large data and databases you will disagree with how I've outlined or described any points here. However I'm not a trained computer scientist, but a biologist, and what I've outlined here has worked for me as I've tried to get my analysis done.

[ar]: Insert active record link
[database]: blog post about using a database
[ree]: ruby enterprise edition
[jruby]: jruby link
[ruby19]: ruby 1.9
[csv]: ruby 1.9 csv link
[merge]: Insert r link
[premature]: premature code optimisation
[batches]: link to find in batches
[ilya]: cite ilya - check his name too
