My single recommendation for any computational biologist is to learn to use a relational database, with a corresponding object relational mapping system (ORM). This sounds complicated, but let me explain.

In bioinformatics, data are distributed as files. Supplementary data are available from journal websites, and a data file is easy to email to a collaborator. Apart from these cases, the use of files in programmes should be limited where ever possible. A bioinformatics project should instead be built on the use of a database.

Using a database allows all data to be accessed in the same way, whether in a script, at the command line, or through third-party software. Databases are fast and optimised for searching data, and for joining datasets together. Joins that would be difficult based on merging files are made much easier through using a relationships in a database.

A simple database workflow first loads all data into the database. Each file can usually become a table in the database. Analytical scripts can then make database calls to pull and join different data sets together. Adding indices to a database futher increases the speed at which joiins are made and data searched.

In comparison using files as the base of the project first results in errors when file paths change. Scripts need rewriting if file formats change. If the data file has a missing bracket or comma, the resulting script will throw an exception and break. The worst thing about using flat files though, is that they must be parsed and joined at the start of each script which is cumbersome and leads to duplication of code across scripts.

### SQL is hard

What I haven't mentioned is learing to use a database is hard. Understanding how to structure tables and the language to join them takes time. Furthermore, writing SQL join statements in scripts requires joining sets of strings together to create the SQL query, which is complex, hard to maintain, and results in ugly code.

Using object relational mapping (ORM) makes using a database easy, and the analytical code simpler and better to understand. Object relational mapping is jargon for what essentially allows rows in a database to be treated as variables in code. This combines the best of efficient data storage, with the familiarity of programming in the language you are used to.
