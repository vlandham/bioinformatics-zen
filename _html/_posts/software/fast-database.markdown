Here I'm going to write some of my experiences working with a bioinformatics project with a large amount of data to process and manage. Maybe some of these tips could be useful for other people doing similar sorts of work. My project involved loading in some biological data, analysing this data, then printing out results for plotting or tabulating. I'll assume that this is in some way relevant to how you may be doing your research. The reason I'm writing this is because working with very large datasets can take a long time to process and then slow down the amount of time taken to get the results that you need. The tips I'm going to give here are ordered from the least technical to more technical. These were the solutions I used, these might not be best but these managed to reduce my process time from a days into hours.

## 0. Use someone else's data

My very first tip is not really a tip at all, which is why it's numbered 0. Get precomputed data already, don't repeat some analysis if somebody else has already done it. Get the data that somebody else has already generated then skip ahead to the next step in research. The best way to save time in analysis is not to do the analysis at all and get the data from else willing to share it.

## 1. Use a bigger computer

This might not sound like a very good solution. For example if you have long tortuous code that is not optimal. This is the easiest to do though. If you're running your code on your desktop computer, try getting hold of a faster computer in the department maybe someone has a bigger tower or there is a department server. More ram and computing power can save a lot of time. The reason to do this is because if the software runs on your computer in should the same on another computer but faster. Use unit testing to make sure though. Deploying and running code on other computers is also very easy with tools like Capistrano. This skips any of the inherent problems of modifying your code to get it to run faster.

## 2. Use a faster language interpreter

I was running my analysis using ruby 1.8.7, and there are many improvements for the ruby interpreter. Such as REE, JRuby and Ruby 1.9. I ended up writing my code on Ruby 1.9 and this improved the execution time significantly and only involved me rewriting a few lines of code to fix a change in the Ruby CSV library. Again this is a quick and easy fix which can save a lot of time.

## 3. Add database indices

Assuming that data is being looked in a database, by an index to rows that are being searched this can speed up code execution time. This is relatively easy to implement and can save a large amount of time depending on how much the database is being queried or tables are being joined.

## 4. Delete data and analysis

I found that during my research that I computed results that I believed might be useful for future analysis. And the best way to save time is was to delete these analysis and only add them when they become necessary. This removes the time required to compute these analysis, and most likely in future these analyses won't be required. The added benefit of doing this is that also removes complexity from the project. 

## 5. Remove database joins

It may be the case that two dataset are compared through the database. The question I asked myself was "Do I need both these variables in my database?". Eventually I found that the answer was no. I could drop one variable from the database and instead move the join further up my workflow and cut some database joins from printing my plotting data. R has a very good merge function. Previously I had completely denormalised all the attributes in my database, but instead I dropped one database table and when I was printing my data so I plot it, I was cutting a database join on one 1.5 million row sized table. Which saved a lot of time, I instead joined my second variable in R after I had calculated the mean of the first data and there was less rows to join.

## 6. Optimise the code

This should be the last step in a software project because the enemy of good-enough code is perfect code. You often find that when you start optimising code you keep doing this to try and shave a few seconds off the code each time. But software doesn't need to be as fast as possible it just needs to be fast enough to produce the results you need in a reasonable amount of time. So don't spend on this, just try and get large chunks of saving if possible. Importantly benchmarking code should be combined with unit testing code to ensure that the optimising the code doesn't break the code. It's better to produce the right results slowly than the wrong results really quickly.

### 6A. Batch load database queries

The first and easiest one is to batch load database queries. This means if you're integrating a series of records in a database, don't pull all the records into memory at the same time. Instead using primary key or database curses, and pull a set of records into memory each time. When pulling all the database records into memory you might find that most of the process time spent loading the database records, rather than actually processing them. A example using ruby is find_in_batches which makes it very easy to perform this analysis. 

### 6B. Association loading

It might be the case that records in one table are associated with records in a foreign table. Association loading means that when you pull the rows out in Table A a single lookup is required to pull all the associated rows out in Table B at the same time. This can save a time because it means that there are less calls being made to the database.

### 6C. Order of loops

I found that this was a very big time sink in my project where I had loops within loops that were querying the database. By moving the database calls further outside the loop order, or out of the loops completely and cache them before the loops I can save time through calling the database less. It's also faster to look cached variables up in memory than querying them from the database.

### 6D. Use raw SQL to manipulate the database

During my project I was using ActiveRecord as the ORM to access my database. However by skipping the ORM and using raw SQL this was able to save a lot of time for me. This is especially the case for inserting millions of rows into the database. I got the idea for this from Ilya Grigriks blog.

## 7. Use mature solutions

This is not really a tip but useful. A lot of Ruby blogs talk about document based schema-less databases like couch db and mongo mapper. I'm sure at some point in the future these may replace relational databases. But new libraries can be very immature and ready to slot into a project. I found that I when I tried this I spent a significant amount of time maintaining these libraries rather then moving onto the analysis I needed to produce. In the end I switched back to vanilla active record because I was spending time getting these new libraries to work how I was used to active record working instead of producing the results I needed with good-enough libraries.

Another example is when I tried using map reduce. A map reduce approach to data analysis receives a lot of attention on blogs of how computation can be done. I'm not arguing this but I found that map reduce is not yet at the stage where it can but not at the stage where it is plugin and play. But again I had to spend time maintaining my software to run with the map reduce libraries, and some existing libraries weren't already compatible with ActiveRecord. Ultimately at the end of the day I'm just a one man show with a limited time to produce results. Programs like Hadoop are likely to be the future and university systems will be designed around this, but there's also a lot to be said for a big server, the latest language interpreter, and a well indexed database.
