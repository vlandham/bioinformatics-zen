---
  category: r_programming
  layout: default
  permalink: /r_programming/data_analysis_using_r_functions_as_objects
---

The R language is useful language to use because statistical and plotting functions are the core of the language. However before using these functions you need to get your input data into the correct format for the function you need. The problem is that R's methods for accessing and subsetting can quickly become complex and confusing.

When you want to manipulate a data frame is someway, for example to for find the mean for specific subsets of the data, this generally means creating variables to keep track of the a series of levels to subset the data by, as well as variables to contain the subsets. These types of loose variables can lead to mistakes as well as making complex R scripts that are difficult to return to. R has an interesting feature that allows treating functions that can remove much of these loose variables. When I write "functions as objects" I mean that the function for calculating standard deviation can be treated in the same was the array of data it works on.

Here I'm going to give some examples of how the "functions as objects" part of R can make data analysis easier to write and read. I'm using the crab data for these examples which are part of the MASS package. This data contains morphological measurements on both sexes on two crab species.

{% highlight r %}

library(MASS)
data(crabs)
head(crabs)
   sp sex index   FL  RW   CL   CW  BD
1   B   M     1  8.1 6.7 16.1 19.0 7.0
2   B   M     2  8.8 7.7 18.1 20.8 7.4
3   B   M     3  9.2 7.8 19.0 22.4 7.7
4   B   M     4  9.6 7.9 20.1 23.1 8.2
5   B   M     5  9.8 8.0 20.3 23.0 8.2

{% endhighlight %}

### Performing functions on subsets of data

I have a function which I want to use to analyse two columns of the crab data but subsetting for species and sex for each function call. In the first example below I'm going I'm getting one subset of the data for the orange female crabs. I then call my function on this subset.

{% highlight r %}

  do_something <- function(x,y){
    # Several lines of complicated function go here ...
  }

  # Subset my data
  orange_girls <- subset(crabs, sex == 'F' & sp == 'O')

  # Call my function
  do_something(orange_girls$CW,orange_girls$CL)

{% endhighlight %}

I don't particularly like this because I have to subset the data first before I call my function on it. Secondly when I call the *do_something* function I have to specify the the columns in data using the *$* index. Now compare the same functionality but using the "with" command.

{% highlight r %}

  with(subset(crabs, sex == 'F' & sp == 'O'), do_something(CW,CL))

{% endhighlight %}

Here I doing exactly the same thing, but the *do_something* command called in the context of the data subset so I don't have to specify which columns I mean using *$*. I also don't have to subset the dataset before, I can just pass this as the first argument to the *with* command. I'm not restricted to just running one function either; by using curly braces *{ }* I can call multiple functions on the subset of the data.

{% highlight r %}

  # Same thing but using a multi-line code block via curly braces
  with(subset(crabs, sex == 'F' & sp == 'O'),{
    do_something(CW,CL)

    # Use more functions on the same subset
    lm(CW ~ CL)
  })

{% endhighlight %}

### Looping through subsets of data

In the example above I'm using just a single subset of the crabs data, but usually I want to call my function on all combinations of sex and species. Usually this uses *for* loops to iterate through levels I'm interested in subsetting the data by.

{% highlight r %}

  # Arrays of values for species and sex
  species <- unique(crabs$sp)
  sexes   <- unique(crabs$sex)

  # Loop through species ...
  for(i in 1:length(species)){

    # ... loop through sex ..
    for(j in 1:length(sexes)){

      # ... and finally call a function on each subset
      something_else(subset(crabs, sp == species[i] & sex == sexes[j]))
    }
  }

{% endhighlight %}

This is messy because I've created to two arrays to keep track of the unique values of each crab species and sex type. Furthermore I've got the values of *i* and *j* created in my environment which if use loops elsewhere using variables with the same names are likely to cause problems. If when you look at this code 
 there are nine lines of script just to keep track of which subset I'm trying to find, but only one actually calling the function I'm interested in. Instead of these for loops I'm going to create my own function to automatically iterate through the dataset and call my function on each dataset.

{% highlight r %}

  each <- function(.column,.data,.lambda){
    
    # Find the column index from it's name
    column_index  <- which(names(.data) == .column)

    # Find the unique values in the column
    column_levels <- unique(.data[,column_index])

    # Loop over these values
    for(i in 1:length(column_levels)){

      # Subset the data and call the passed function on it
      .lambda(.data[.data[,column_index] == column_levels[i],])  
    }
  }

{% endhighlight %}

The first two arguments of this function are the name of the column I want to subset the data by, and the data frame I want to iterate over. What's interesting though is the last argument *.lambda* which is not a string or a data frame but an R function. The reason I can do this is because R treats functions as objects  and allows them as arguments to other functions.

{% highlight r %}

  # Another function as the last argument to this function
  each("sp", crabs, something_else)

  # Or create a new anonymous function ...
  each("sp", crabs, function(x){

    # ... and run multiple lines of code here
    something_else(x)
    with(x,lm(CW ~ CL))

  })

{% endhighlight %}

Here I'm looping through each subset of the data and I've moved all the messy code associated with looping through the dataset to a function I can call repeatedly. Moving commonly called code to a separate function is good programming practice as it keeps your code [DRY][dry] and also avoid the temptation for [cut and paste anti-patterns][cap].

My example function isn't very sophisticated though and can't create loops for subsets of more than one column. Nevertheless as you might expect there are methods exactly for doing this type of loop-each-subset approach to data analysis. An example is lapply function in R base, however I find the syntax of this function quite hard to remember. Instead I prefer to use [Hadly Wickham's][hadly] excellent [plyr package][plyr]. Plyr is very simple to use, for example:

{% highlight r %}

  # Need to install plyr from cran
  library(plyr)

  # Three arguments
  # 1. The dataframe
  # 2. The name of columns to subset by
  # 3. The function to call on each subset
  d_ply(crabs, .(sp, sex), something_else)

{% endhighlight %}

Compare this with the code a few paragraphs up which uses for loops to do the same thing. The plyr package has more functionality than this though and is worth spending some time looking at. Also worth checking out are the [foreach][foreach] and [iterators][iter] packages which provide related functionality.

[dry]: http://en.wikipedia.org/wiki/Don't_repeat_yourself
[cap]: http://en.wikipedia.org/wiki/Copy_and_paste_programming
[hadly]: http://had.co.nz/
[plyr]: http://had.co.nz/plyr/
[foreach]: cran.r-project.org/web/packages/foreach/index.html
[iter]: http://cran.r-project.org/web/packages/iterators/index.html
