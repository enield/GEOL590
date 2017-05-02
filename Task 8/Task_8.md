Load in librarys:

``` r
library(tidyverse)
```

    ## Loading tidyverse: ggplot2
    ## Loading tidyverse: tibble
    ## Loading tidyverse: tidyr
    ## Loading tidyverse: readr
    ## Loading tidyverse: purrr
    ## Loading tidyverse: dplyr

    ## Conflicts with tidy packages ----------------------------------------------

    ## filter(): dplyr, stats
    ## lag():    dplyr, stats

``` r
library(microbenchmark)
```

    ## Warning: package 'microbenchmark' was built under R version 3.3.3

Functions
---------

**Objective:** Write a function that fulfills the following criteria:

1.  It should be tidyverse compatible (i.e., the first argument must be a data frame)

2.  It should add two arbitrary columns of the data frame (specified by the user) and put them in a new column of that data frame, with the name of the new column specified by the user

3.  It should throw an informative warning if any invalid arguments are provided. Invalid arguments might include: -The first argument is not a data frame -Less than two valid columns are specified to add (e.g., one or both of the column names isn't in the supplied data frame) -The columns specified are not numeric, and therefore can't be added - use tryCatch() for this -If the columns to add aren't valid but the new column name is, the function should create a column of NA values

Start by creating a data frame with two columns:

``` r
mydata<-tibble::tibble(
  a=c(2,4,6,8,10),
  b=c(1,1,1,1,1)
)
head(mydata)
```

    ## # A tibble: 5 × 2
    ##       a     b
    ##   <dbl> <dbl>
    ## 1     2     1
    ## 2     4     1
    ## 3     6     1
    ## 4     8     1
    ## 5    10     1

Now create the code that will add the two columns in the data frame and place them into a new column called 'c'.

``` r
mydata$c<-mydata$a+mydata$b #subsetted each of the columns using $ symbol, defined the name of the new column at the beginning
head(mydata)
```

    ## # A tibble: 5 × 3
    ##       a     b     c
    ##   <dbl> <dbl> <dbl>
    ## 1     2     1     3
    ## 2     4     1     5
    ## 3     6     1     7
    ## 4     8     1     9
    ## 5    10     1    11

Can we simplify this function? Yes!

How many inputs does this function have? Two, col a and col b. Rename them to make it easier to understand the arguments in the function.

``` r
df_input<-mydata
x<-df_input$a
y<-df_input$b
```

Now rewrite the code you used to add the two columns together using the arguments you just defined (x,y and the data frame they came from)

``` r
df_input$name<-x+y #adding a new col called name
```

Write a function called addcol (did not give it the name add\_column because this function already exsits), let the arguments be the names of the columns you want to add. Define the data frame you will be using as df\_input. Define the name of the new col in the 'name' argument.

``` r
addcol<-function(df_input,x,y,name){
  col1<-df_input$x
  col2<-df_input$y
  df_input$name<-col1+col2
}
```

Test the code with a new data frame.

``` r
#create new dataframe called 'data'
data2<-tibble::tibble(
  d=c(2,4,6,8,10),
  e=c(1,1,1,1,1),
  f=c("e","m","i","l","y")
)
#test out new function
#data<-addcol(data,d,e,new)
```

*That should have worked. Ask Drew why it didn't work.*
-------------------------------------------------------

Option 2: Create a new value where the 2 coloumns are added together and then mutate it on to the data frame.

``` r
addcol2 <- function(dataframe, col1, col2, name){
  dataframe2 <- mutate(dataframe, name=col1+col2)
  return(dataframe2)
  
}
#Test with data2 dataframe
addcol2(data2, data2$d, data2$e, new)
```

    ## # A tibble: 5 × 4
    ##       d     e     f  name
    ##   <dbl> <dbl> <chr> <dbl>
    ## 1     2     1     e     3
    ## 2     4     1     m     5
    ## 3     6     1     i     7
    ## 4     8     1     l     9
    ## 5    10     1     y    11

Great I have a working function. Add in an error message. Add a message if you are trying to add a text string to a number.

``` r
addcol3 <- function(dataframe, col1, col2, name){
  
  if(is_character(col1)!= TRUE)
    warning("can't add apples to oranges, first column is not numeric") 
  
  if(is_character(col2)!= TRUE)
    warning("can't add apples to oranges, second column is not numeric") 
  
  dataframe2 <- mutate(dataframe, name=col1+col2)
  return(dataframe2)
  
}
```

Test the error message in the final function:

``` r
#you added a character column when you first created the data2 data frame, now use that col.

#run function with character frame
#addcol3(data2,data2$d,data2$f,name)
```

Loop and performance metric tasks
---------------------------------

**Objective: Write a function named that uses a for loop to calculate the sum of the elements of a vector, which is passed as an argument (i.e., it should do the same thing that sum() does with vectors). your\_fun(1:10^4) should return 50005000.**

``` r
loop_sum <- function(x){

n <- as.numeric(length(x)) #loop works best with numeric numbers

loop_sum= 0

for (i in 1:n) {
  loop_sum <- loop_sum + x[i]
               }

print(loop_sum)

}

#test function
loop_sum(1:10^4)
```

    ## [1] 50005000

**Objective: Use the microbenchmark::microbenchmark function to compare the performace of your function to that of sum in adding up the elements of the vector 1:10^4. Is there a difference? Why? The benchmarking code should look something like:**

``` r
test.vec <- 1:10^4
microbenchmark(
    loop_sum(test.vec),
    sum(test.vec)
    )
```

    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000
    ## [1] 50005000

    ## Unit: microseconds
    ##                expr      min       lq       mean    median       uq
    ##  loop_sum(test.vec) 4322.768 4509.830 5046.03172 4825.8795 5481.485
    ##       sum(test.vec)    5.531    5.532    6.38129    5.9275    6.717
    ##       max neval
    ##  6873.288   100
    ##    15.013   100

Look like my function loop\_sum takes longer to fun than the built in sum function. Probably because of the for loop.
