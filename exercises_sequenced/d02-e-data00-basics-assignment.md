Data Basics
================
Zach del Rosario
2020-05-03

# Data: Basics

*Purpose*: When first studying a new dataset, there are very simple
checks we should perform first. These are those checks.

Additionally, we’ll have our first look at the *pipe operator*, which
will be super useful for writing code that’s readable.

*Reading*: (None)

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.4.2

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

## First Checks

<!-- -------------------------------------------------- -->

### **q0** Run the following chunk:

*Hint*: You can do this either by clicking the green arrow at the
top-right of the chunk, or by using the keybaord shortcut `Shift` +
`Cmd/Ctrl` + `Enter`.

``` r
head(iris)
```

    ##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
    ## 1          5.1         3.5          1.4         0.2  setosa
    ## 2          4.9         3.0          1.4         0.2  setosa
    ## 3          4.7         3.2          1.3         0.2  setosa
    ## 4          4.6         3.1          1.5         0.2  setosa
    ## 5          5.0         3.6          1.4         0.2  setosa
    ## 6          5.4         3.9          1.7         0.4  setosa

This is a *dataset*; the fundamental object we’ll study throughout this
course. Some nomenclature:

- The `1, 2, 3, ...` on the left enumerate the **rows** of the dataset
- The names `Sepal.Length`, `Sepal.Width`, `...` name the **columns** of
  the dataset
- The column `Sepal.Length` takes **numeric** values
- The column `Species` takes **string** values

### **q1** Load the `tidyverse` and inspect the `diamonds` dataset. What do the

`cut`, `color`, and `clarity` variables mean?

*Hint*: You can run `?diamonds` to get information on a built-in
dataset.

### **q2** Run `glimpse(diamonds)`; what variables does `diamonds` have?

### **q3** Run `summary(diamonds)`; what are the common values for each of the

variables? How widely do each of the variables vary?

*Hint*: The `Median` and `Mean` are common values, while `Min` and `Max`
give us a sense of variation.

``` r
summary(diamonds)
```

    ##      carat               cut        color        clarity          depth      
    ##  Min.   :0.2000   Fair     : 1610   D: 6775   SI1    :13065   Min.   :43.00  
    ##  1st Qu.:0.4000   Good     : 4906   E: 9797   VS2    :12258   1st Qu.:61.00  
    ##  Median :0.7000   Very Good:12082   F: 9542   SI2    : 9194   Median :61.80  
    ##  Mean   :0.7979   Premium  :13791   G:11292   VS1    : 8171   Mean   :61.75  
    ##  3rd Qu.:1.0400   Ideal    :21551   H: 8304   VVS2   : 5066   3rd Qu.:62.50  
    ##  Max.   :5.0100                     I: 5422   VVS1   : 3655   Max.   :79.00  
    ##                                     J: 2808   (Other): 2531                  
    ##      table           price             x                y         
    ##  Min.   :43.00   Min.   :  326   Min.   : 0.000   Min.   : 0.000  
    ##  1st Qu.:56.00   1st Qu.:  950   1st Qu.: 4.710   1st Qu.: 4.720  
    ##  Median :57.00   Median : 2401   Median : 5.700   Median : 5.710  
    ##  Mean   :57.46   Mean   : 3933   Mean   : 5.731   Mean   : 5.735  
    ##  3rd Qu.:59.00   3rd Qu.: 5324   3rd Qu.: 6.540   3rd Qu.: 6.540  
    ##  Max.   :95.00   Max.   :18823   Max.   :10.740   Max.   :58.900  
    ##                                                                   
    ##        z         
    ##  Min.   : 0.000  
    ##  1st Qu.: 2.910  
    ##  Median : 3.530  
    ##  Mean   : 3.539  
    ##  3rd Qu.: 4.040  
    ##  Max.   :31.800  
    ## 

**Observations**:

- (Write your observations here!)

You should always analyze your dataset in the simplest way possible,
build hypotheses, and devise more specific analyses to probe those
hypotheses. The `glimpse()` and `summary()` functions are two of the
simplest tools we have.

## The Pipe Operator

<!-- -------------------------------------------------- -->

Throughout this class we’re going to make heavy use of the *pipe
operator* `%>%`. This handy little function will help us make our code
more readable. Whenever you see `%>%`, you can translate that into the
word “then”. For instance

``` r
diamonds %>%
  group_by(cut) %>%
  summarize(carat_mean = mean(carat))
```

    ## # A tibble: 5 × 2
    ##   cut       carat_mean
    ##   <ord>          <dbl>
    ## 1 Fair           1.05 
    ## 2 Good           0.849
    ## 3 Very Good      0.806
    ## 4 Premium        0.892
    ## 5 Ideal          0.703

Would translate into the tiny “story”

- Take the `diamonds` dataset, *then*
- Group it by the variable `cut`, *then*
- summarize it by computing the `mean` of `carat`

*What the pipe actually does*. The pipe operator `LHS %>% RHS` takes its
left-hand side (LHS) and inserts it as an the first argument to the
function on its right-hand side (RHS). So the pipe will let us take
`glimpse(diamonds)` and turn it into `diamonds %>% glimpse()`.

### **q4** Use the pipe operator to re-write `summary(diamonds)`.

## Reading Data

<!-- -------------------------------------------------- -->

So far we’ve only been looking at built-in datasets. Ultimately, we’ll
want to read in our own data. We’ll get to the art of loading and
*wrangling* data later, but for now, know that the `readr` package
provides us tools to read data. Let’s quickly practice loading data
below.

### **q5** Use the function `read_csv()` to load the file `"./data/tiny.csv"`.

``` r
##df_q5
```

<!-- include-exit-ticket -->

# Exit Ticket

<!-- -------------------------------------------------- -->

Once you have completed this exercise, make sure to fill out the **exit
ticket survey**, [linked
here](https://docs.google.com/forms/d/e/1FAIpQLSeuq2LFIwWcm05e8-JU84A3irdEL7JkXhMq5Xtoalib36LFHw/viewform?usp=pp_url&entry.693978880=e-data00-basics-assignment.Rmd).
