Data: Deriving Quantities
================
Zachary del Rosario
2020-05-07

# Data: Deriving Quantities

*Purpose*: Often our data will not tell us *directly* what we want to
know; in these cases we need to *derive* new quantities from our data.
In this exercise, we’ll work with `mutate()` to create new columns by
operating on existing variables, and use `group_by()` with `summarize()`
to compute aggregate statistics (summaries!) of our data.

*Reading*: [Derive Information with
dplyr](https://rstudio.cloud/learn/primers/2.3) *Topics*: (All topics,
except *Challenges*) *Reading Time*: ~60 minutes

*Note*: I’m considering splitting this exercise into two parts; I
welcome feedback on this idea.

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

### **q1** What is the difference between these two versions? How are they the same? How are they different?

``` r
## Version 1
filter(diamonds, cut == "Ideal")
```

    ## # A tibble: 21,551 × 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <ord> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.23 Ideal E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2  0.23 Ideal J     VS1      62.8    56   340  3.93  3.9   2.46
    ##  3  0.31 Ideal J     SI2      62.2    54   344  4.35  4.37  2.71
    ##  4  0.3  Ideal I     SI2      62      54   348  4.31  4.34  2.68
    ##  5  0.33 Ideal I     SI2      61.8    55   403  4.49  4.51  2.78
    ##  6  0.33 Ideal I     SI2      61.2    56   403  4.49  4.5   2.75
    ##  7  0.33 Ideal J     SI1      61.1    56   403  4.49  4.55  2.76
    ##  8  0.23 Ideal G     VS1      61.9    54   404  3.93  3.95  2.44
    ##  9  0.32 Ideal I     SI1      60.9    55   404  4.45  4.48  2.72
    ## 10  0.3  Ideal I     SI2      61      59   405  4.3   4.33  2.63
    ## # ℹ 21,541 more rows

``` r
## Version 2
diamonds %>% filter(cut == "Ideal")
```

    ## # A tibble: 21,551 × 10
    ##    carat cut   color clarity depth table price     x     y     z
    ##    <dbl> <ord> <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
    ##  1  0.23 Ideal E     SI2      61.5    55   326  3.95  3.98  2.43
    ##  2  0.23 Ideal J     VS1      62.8    56   340  3.93  3.9   2.46
    ##  3  0.31 Ideal J     SI2      62.2    54   344  4.35  4.37  2.71
    ##  4  0.3  Ideal I     SI2      62      54   348  4.31  4.34  2.68
    ##  5  0.33 Ideal I     SI2      61.8    55   403  4.49  4.51  2.78
    ##  6  0.33 Ideal I     SI2      61.2    56   403  4.49  4.5   2.75
    ##  7  0.33 Ideal J     SI1      61.1    56   403  4.49  4.55  2.76
    ##  8  0.23 Ideal G     VS1      61.9    54   404  3.93  3.95  2.44
    ##  9  0.32 Ideal I     SI1      60.9    55   404  4.45  4.48  2.72
    ## 10  0.3  Ideal I     SI2      61      59   405  4.3   4.33  2.63
    ## # ℹ 21,541 more rows

The reading mentioned various kinds of *summary functions*, which are
summarized in the table below:

## Summary Functions

| Type     | Functions                                            |
|----------|------------------------------------------------------|
| Location | `mean(x), median(x), quantile(x, p), min(x), max(x)` |
| Spread   | `sd(x), var(x), IQR(x), mad(x)`                      |
| Position | `first(x), nth(x, n), last(x)`                       |
| Counts   | `n_distinct(x), n()`                                 |
| Logical  | `sum(!is.na(x)), mean(y == 0)`                       |

### **q2** Using `summarize()` and a logical summary function, determine the number of rows with `Ideal` `cut`. Save this value to a column called `n_ideal`.

``` r
df_q2 <-
  diamonds %>% 
  summarize(n_ideal = sum(cut == "Ideal"))
df_q2
```

    ## # A tibble: 1 × 1
    ##   n_ideal
    ##     <int>
    ## 1   21551

The following test will verify that your `df_q2` is correct:

``` r
## NOTE: No need to change this!
assertthat::assert_that(
  assertthat::are_equal(
    df_q2 %>% pull(n_ideal),
    21551
  )
)
```

    ## [1] TRUE

``` r
print("Great job!")
```

    ## [1] "Great job!"

### **q3** The function `group_by()` modifies how other dplyr verbs function. Uncomment the `group_by()` below, and describe how the result changes.

``` r
diamonds %>%
 group_by(color, clarity) %>%
  summarize(price = mean(price))
```

    ## `summarise()` has grouped output by 'color'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 56 × 3
    ## # Groups:   color [7]
    ##    color clarity price
    ##    <ord> <ord>   <dbl>
    ##  1 D     I1      3863.
    ##  2 D     SI2     3931.
    ##  3 D     SI1     2976.
    ##  4 D     VS2     2587.
    ##  5 D     VS1     3030.
    ##  6 D     VVS2    3351.
    ##  7 D     VVS1    2948.
    ##  8 D     IF      8307.
    ##  9 E     I1      3488.
    ## 10 E     SI2     4174.
    ## # ℹ 46 more rows

### Vectorized Functions

| Type            | Functions                                                                            |
|-----------------|--------------------------------------------------------------------------------------|
| Arithmetic ops. | `+, -, *, /, ^`                                                                      |
| Modular arith.  | `%/%, %%`                                                                            |
| Logical comp.   | `<, <=, >, >=, !=, ==`                                                               |
| Logarithms      | `log(x), log2(x), log10(x)`                                                          |
| Offsets         | `lead(x), lag(x)`                                                                    |
| Cumulants       | `cumsum(x), cumprod(x), cummin(x), cummax(x), cummean(x)`                            |
| Ranking         | `min_rank(x), row_number(x), dense_rank(x), percent_rank(x), cume_dist(x), ntile(x)` |

### **q4** Comment on why the difference is so large.

The `depth` variable is supposedly computed via
`depth_computed = 100 * 2 * z / (x + y)`. Compute
`diff = depth - depth_computed`: This is a measure of discrepancy
between the given and computed depth. Additionally, compute the
*coefficient of variation* `cov = sd(x) / mean(x)` for both `depth` and
`diff`: This is a dimensionless measure of variation in a variable.
Assign the resulting tibble to `df_q4`, and assign the appropriate
values to `cov_depth` and `cov_diff`. Comment on the relative values of
`cov_depth` and `cov_diff`; why is `cov_diff` so large?

*Note*: Confusingly, the documentation for `diamonds` leaves out the
factor of `100` in the computation of `depth`.

``` r
## TODO: Compute the `cov_depth` and `cov_diff` and assign the result to df_q4
df_q4 <-
  diamonds %>% 
  mutate(
  depth_computed = 100 * 2 * z /(x + y),
  diff = depth - depth_computed
  ) %>% 
  summarize(
    depth_mean = mean(depth, na.rm = TRUE),
    depth_sd = sd(depth, na.rm = TRUE),
    cov_depth = depth_sd/depth_mean,
    
    diff_mean = mean(diff, na.rm = TRUE),
    diff_sd = sd(diff, na.rm = TRUE),
    cov_diff = diff_sd/diff_mean,
    c_diff = IQR(diff, na.rm = TRUE)/median(diff, na.rm = TRUE)
  
            )
df_q4
```

    ## # A tibble: 1 × 7
    ##   depth_mean depth_sd cov_depth diff_mean diff_sd cov_diff   c_diff
    ##        <dbl>    <dbl>     <dbl>     <dbl>   <dbl>    <dbl>    <dbl>
    ## 1       61.7     1.43    0.0232   0.00528    2.63     498. -7.50e12

**Observations**

- Comment on the relative values of `cov_depth` and `cov_diff`.
- Why is `cov_diff` so large?

Depth & depth computed is small

The following test will verify that your `df_q4` is correct:

``` r
## NOTE: No need to change this!
assertthat::assert_that(abs(df_q4 %>% pull(cov_depth) - 0.02320057) < 1e-3)
```

    ## [1] TRUE

``` r
assertthat::assert_that(abs(df_q4 %>% pull(cov_diff) - 497.5585) < 1e-3)
```

    ## [1] TRUE

``` r
print("Nice!")
```

    ## [1] "Nice!"

There is nonzero difference between `depth` and the computed `depth`;
this raises some questions about how `depth` was actually computed! It’s
often a good idea to try to check derived quantities in your data, if
possible. These can sometimes uncover errors in the data!

### **q5** Compute and observe

Compute the `price_mean = mean(price)`, `price_sd = sd(price)`, and
`price_cov = price_sd / price_mean` for each `cut` of diamond. What
observations can you make about the various cuts? Do those observations
match your expectations?

``` r
## TODO: Assign result to df_q5
df_q5 <-
  diamonds %>% 
  group_by(cut) %>% 
  summarise(
    price_mean = mean(price),
    price_sd = sd(price),
    price_cov = price_sd/price_mean
  )
df_q5
```

    ## # A tibble: 5 × 4
    ##   cut       price_mean price_sd price_cov
    ##   <ord>          <dbl>    <dbl>     <dbl>
    ## 1 Fair           4359.    3560.     0.817
    ## 2 Good           3929.    3682.     0.937
    ## 3 Very Good      3982.    3936.     0.988
    ## 4 Premium        4584.    4349.     0.949
    ## 5 Ideal          3458.    3808.     1.10

The following test will verify that your `df_q5` is correct:

``` r
## NOTE: No need to change this!
assertthat::assert_that(
  assertthat::are_equal(
    df_q5 %>%
      select(cut, price_cov) %>%
      mutate(price_cov = round(price_cov, digits = 3)),
    tibble(
      cut = c("Fair", "Good", "Very Good", "Premium", "Ideal"),
      price_cov = c(0.817, 0.937, 0.988, 0.949, 1.101)
    ) %>%
    mutate(cut = fct_inorder(cut, ordered = TRUE))
  )
)
```

    ## [1] TRUE

``` r
print("Excellent!")
```

    ## [1] "Excellent!"

**Observations**:

- Write your observations here!

<!-- include-exit-ticket -->

# Exit Ticket

<!-- -------------------------------------------------- -->

Once you have completed this exercise, make sure to fill out the **exit
ticket survey**, [linked
here](https://docs.google.com/forms/d/e/1FAIpQLSeuq2LFIwWcm05e8-JU84A3irdEL7JkXhMq5Xtoalib36LFHw/viewform?usp=pp_url&entry.693978880=e-data02-derive-assignment.Rmd).
