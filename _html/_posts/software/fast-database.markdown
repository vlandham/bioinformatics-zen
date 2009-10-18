Computational research usually a involves a fair amount of shuffling data into the right format so that it can be plotted or tested using statistics. I prefer to [use a database to store and format data][database] as I think this make projects easier to maintain compared with lots of little formatting scripts. I find that using of a dynamic language like Ruby and database libraries like [ActiveRecord][ar] makes using a database in research relatively simple.

Using a database stops being simple when you have to manipulate very large amounts of data, because tying to manipulate big data in the same as way as small data leads to analyses taking days. Here I'm going to write about my experience of doing research with gigabytes of data with millions of data points. The methods I'm outlining here are all things I tried and I've ordered them from what I think is the least to most technical.

## The simple things

### 1. Use existing data

Not really a tip, but if you can get the data you need from someone else then you don't even need to run any analysis. This may seem obvious but I know from experience it is in a new research project to sit down and start coding. Nevertheless an effective way to save time and effort is to rely on existing data rather than jumping in head first with your own code and analysis.

### 2. Use a bigger computer

Using a faster computer might seem like a lazy option compared with optimising your code, if the analysis works on your computer it should work the same, but faster, on a better computer. Using a faster computer is probably one for the few things you can try which which doesn't involved modifying your code and therefore should not introduce any bugs. Nevertheless unit tests should be used to make sure the code still works as expected. 

### 3. Add database indices

Since I'm using a database for my analysis making sure the database runs as fast as possible is important. Properly indexed database columns can reducing running when searching or joining tables since the corresponding rows are looked up much faster. Database indices are relatively easy, just specify which columns need to be indexed either using SQL or the ORM.

### 4. Use a faster language interpreter

Most of the time I find that using standard version of ruby that comes on a Mac is fine. In last two years though different but faster versions of Ruby have been created these include [REE][ree], [JRuby][jruby] and [Ruby 1.9][ruby19]. Therefore when I was working with a very large amount of data and I was having problems with running times I though it was worth a try using a new faster version of Ruby. When I tried running my code using Ruby 1.9 I found it improved my run time significantly. Unlike simply running my analysis on a bigger a server, I had to change code for [compatibility with the new Ruby CSV library][csv]. Updating the code was still relatively easy and trying a better language interpreter did return a noticeable benefit.

[ree]: ruby enterprise edition
[jruby]: jruby link
[ruby19]: ruby 1.9
[csv]: ruby 1.9 csv link

## Delete stuff

When the above three points haven't helped enough generally I have to start digging around in the code - which is bad because changing working code can create bugs. A good way to optimise code though is to delete it entirely.

### 5. Delete data and analysis

I find that I often generate variables which I think might be useful at some future time. As you might expect just deleting this saves time and more often than not the variable wasn't even required anyway.

### 6. Remove database table joins

I'm using a database because usually I two or more sets of data and I want to format them in a way that makes them comparable. Once formatted the two variables are joined in the database and printed as text file which I'll use to create a figure.

The problem with this is that with a large number of database records a simple join between, even with indexes, can take a very long time. The amount of time required also increases the more the data is denormalised. To try and remedy this I found that I could drop the smaller of two variables from my database and instead do the data join further into my workflow.

For example I had two sets of data, the first contained millions of entries each one corresponding to a residue a yeast proteins. The second data contained around 100 entries each corresponding to one of the twenty amino acids. When I was joining these two variables in my database I performing millions of joins which took a long time.

So instead I joined my amino acid data to protein data after I had calculated the mean of each amino acid for each protein residue. This reduced the number of joins from a million down to around 100. I did this using the relatively easy to use [merge function in R][merge].

[merge]: Insert r link

## Code optimisation

I think optimising code for performance should be the last resort. The reason being that the enemy of good-enough code is perfect code - when you start optimising code you keep going more than is necessary. The code doesn't need to be as fast as possible though, just fast enough to get the desired results. A second point is that optimising code, means changing code, which can then introduce bugs so the more code is optimised the more chance of introducing bugs. Therefore I think code optimisation should be done sparing and combined with unit testing. It's better to produce the right results slowly than the wrong results quickly.

### 7A. Batch load database query results

The time to process large tables and queries can be reduced by to batch loading the returned database rows into memory. Pulling all the database records into memory (usually unintentionally) can mean that most of the process is time spent loading data into memory rather than actually dealing with it. Pulling subsets of records into memory at a time means that each subset is processed before the next set or rows is retrieved. A example of this in Ruby is [the ActiveRecord method find_in_batches][batches]. 

### 7B. Association loading

Association loading means that when a row of Table A is pulled from the database into memory that all the rows associated with it in Table B are also retrieved at the same time. This will usually mean that the database is only queried twice, once to find the records from Table A and once to get the corresponding records from Table B. The alternative but slower method is to use a loop to retrieve each row in Table B when it's required but this will mean as many database queries as there are rows.

### 7C. Database querying in loops

I found that database querying in large loops can create long lengths of processing time. I tried to improve things by instead moving the database calls, up or out of the loops as much as possible. I cached the rows I needed from the database in memory (assuming it was a small number) before I needed them. Therefore the looping code was looking things up memory rather then querying the database each time. Doing this correctly was one of the things which saved the most amount of processing time for my project.

### 7D. Use raw SQL

Object relational management (ORM) libraries like ActiveRecord allow the database to be seamlessly manipulated using object orientated programming. Using an ORM does however add a performance penalty because it's an extra layer of code on top of the database. When I'm was doing millions of database updates I found that added a lot of processing time compared using raw SQL and skipping using the ORM. The advantages of this technique are neatly outlined [by Ilya Grigorik][ilya].

[ar]: Insert active record link
[database]: blog post about using a database
[batches]: link to find in batches
[ilya]: cite ilya - check his name too
