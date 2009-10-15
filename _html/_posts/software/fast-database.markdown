My research generally involves loading data into a database, shuffling the data, then printing the reordered data for a figure or table. I'll assume that this might be similar to how you do organise your research projects too. I find that using a combination of Ruby and database libraries like [ActiveRecord][ar] makes database backed bioinformatics research easy.

[ar]: Insert active record link

Here I'm going to write about my experience of doing research with gigabytes of data with millions of data points. When working with very big amounts data I found that I manipulate the data in the same as way as a small amount of data because running the analysis just took too long to load. The methods I outlined here are ordered from the less to more technical.

## The simple things

### 1. Use existing data

Not really a tip, but if you can get the data you need from someone else then you can skip ahead to whatever step is next in your project. This may seem obvious but it's tempting when starting a new research project to sit down and start coding. Nevertheless the best way to save time and effort is to rely on existing data (and code libraries) as much as possible.

### 2. Use a bigger computer

Using a faster computer might seem like a lazy option compared with optimising your code. However if the software already works on your computer it should work the same on a bigger, better computer. Unit tests should be used to make sure the code works as expected though. Using a faster computer is probably the only solution which doesn't involved modifying the code or the database and should therefore cannot introduce any bugs.

### 3. Use a faster language interpreter

Most of the time using standard ruby interpreter in OSX is fine, but in last two years there have been different but all faster implementations of the Ruby interpreter such as [REE][ree], [JRuby][jruby] and [Ruby 1.9][ruby19]. When I was having problems running my analysis a large amount of data trying my code using Ruby 1.9 improved run time significantly. Unlike using a bigger a server I had to rewrite a few lines of code for [compatibility with the new Ruby CSV library][csv]. This was still relatively easy to implement and does return a noticeable benefit, as above use unit tests to make sure everything still works as expected.

[ree]: ruby enterprise edition
[jruby]: jruby link
[ruby19]: ruby 1.9
[csv]: ruby 1.9 csv link

### 4. Add database indices

Making sure database columns are properly indexed decrease speed when searching or joining tables. Database indices are also relatively to implement and the amount of time saved depends on how much the database is being queried or tables are being joined.

## Delete stuff

If the above three points haven't helped so far generally you have to start digging around in the code - which is bad because bugs can start appearing. The following two points shouldn't cause too much of a problem though because code is just being removed rather than changed.

### 5. Delete data and analysis

I found that during my research I often calculate results which I though might be useful for at some point in the future. As you might expect just to delete these analysis you remove the time required to compute them. Most likely in future they won't even be required anyway. The added benefit of benefit of deleting code is that is removes bugs along with it. 

### 6. Remove table joins in the database

Usually I'm using a database because I two or more sets of data, and I want to shuffle in a format that makes them comparable. I then compared the my variables my joining them in the database and printing a text file with the result.

With a large number of database records I found that a simple join between two tables could take a long time, and if my data was denormalised the time would increase accordingly. By experimenting I found that I could drop one of my variables from my database and instead join the second variable further along my workflow when there were less records. Instead of joining two large tables in my database, I instead joined my second variable using the [merge function in R][merge] when I was plotting the data and there were less rows required to make the join on.

[merge]: Insert r link

## Code optimisation

I think this should be the last step in a software project. The reason is because the enemy of good-enough code is perfect code, you often find that when you start optimising code you keep going more than is necessary. Software doesn't need to be as fast as possible just needs to be fast enough to get the job done. A second point is that optimising code, means changing code, which can then introduce bugs. Therefore code optimisation should be combined with thorough unit testing to ensure nothing gets broken. It's better to produce the right results slowly than the wrong results quickly.

### 7A. Batch load database query results

An easy way to improve database code is to batch load database queries. If you're doing some analysis to the entries in a database table don't pull all the records into memory at the same time. Pulling all the database records into memory (usually unintentionally) usually means that most of the process is time spent loading data into memory rather than actually processing it. Instead using a primary key or database curses to pull subsets of records into memory at a time. Each subset is processed then the next set pulled out. A example of this in Ruby is the ActiveRecord method find_in_batches which does exactly this. 

### 7B. Association loading

Association loading means that when you pull a row of Table A that all the associated rows in Table B are retrieved at the same time. This will usually mean that the database is only queried twice. The alternative slower solution is to use a loop to pull each foreign table row out when it's required which translates to as many database queries as there are corresponding rows.

### 7C. Variable loading in loops

I found when working with a large database that object creation or database querying in loops can suck up a lot of processor time. I improved things by moving the database calls, up or out of the loops as much as possible. Instead I cached the rows I need from the database in memory before I needed them. Therefore the code was looking things up memory rather then querying the database, which for large loops made a big difference. 

### 7D. Use raw SQL

Object relation management (ORM) libraries like ActiveRecord allow the database to be seamlessly manipulated using object orientated programming. Using an ORM does however add a time penalty because it's an extra layer between your code and the database. In instances where you are performing millions of database insertions or updates using raw SQL and skipping the ORM can save time. This is neatly outlined in [a post on Ilya Grigorik's blog][ilya].

[ilya]: cite ilya - check his name too

## Stick to mature software

Blog often buzz about the latest and the latest software such as schema-less databases like [couchDB][couch] and [mongoDB][mongo], or map-reduce approaches like [Hadoop][hadoop]. I'm sure in the future these technologies will become stand practice. I'm not arguing this but I found that map reduce is not yet at the stage where it can but not at the stage where it is plugin and play. However when working with these I found that I spent a significant amount of time maintaining the code to work with these software rather then moving onto the analysis I needed to produce. In the end I switched back to the solutions I was used to using because I was spending time getting cutting edge software to work instead of producing the results I needed with good-enough software. Ultimately I'm just one man with a limited time to produce results. Schemaless database and map-reduce algorithms are likely to be the future, but there's also a lot to be said in the mean time for a big server, the latest language interpreter, and a database with the right indices.

[couch]: insert couch db link
[mongo]: insert mongo db link
[hadoop]: insert hadoop
