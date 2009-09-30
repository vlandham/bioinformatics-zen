---
  category: r_programming
  layout: default
  permalink: /r_programming/using_code_blocks
---

{% highlight r %}

  do_something <- function(x,y){
    # Several lines of complicated function here ...
  }

  # Not very readable and not DRY
  orange_girls <- subset(crabs, sex == 'F' & sp == 'O')
  do_something(orange_girls$CW,orange_girls$CL)

  blue_boys    <- subset(crabs, sex == 'M' & sp == 'B')
  do_something(blue_boys$CW,blue_boys$CL)


  # Subset data and refer varibles in function
  with(subset(crabs, sex == 'F' & sp == 'O'), do_something(CW,CL))

  # Same thing but using a multiline code block via curly braces
  with(subset(crabs, sex == 'M' & sp == 'B'),{
    do_something(CW,CL)

    # Can add more functions to examine the same data
    lm(CW ~ CL)
  })
{% endhighlight %}

{% highlight r %}

  something_else <- function(df){
    # Something else complicated
  }

  species <- unique(crabs$sp)
  for(i in 1:length(species)){
    species_subset <- subset(crabs, sp == species[i])
    something_else(species_subset)
  }

  # Write a custom function
  each <- function(.column,.data,.lambda){
    index  <- which(names(.data) == .column)
    values <- unique(.data[,index])
    for(i in 1:length(values)){

      # Subset the data and call the passed function on it
      .lambda(.data[.data[,index] == values[i],])  
    }
  }

  # We can now subset the data for each variable type
  each("sp", crabs, something_else(df))
  each("sex",crabs, something_else(df))

  # Plyr offers this functionality and more
  # Also checkout foreach
  library(plyr)
  d_ply(crabs, .(sp, sex), function(arg){

    # My function
    something_else(arg)

    # Call any other functions in this block
  })
{% endhighlight %}
