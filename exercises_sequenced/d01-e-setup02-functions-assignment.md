Setup: Function Basics
================
2020-05-03

# Setup: Function Basics

*Purpose*: Functions are our primary tool in carying out data analysis
with the `tidyverse`. It is unreasonable to expect yourself to memorize
every function and all its details. To that end, we’ll learn some basic
*function literacy* in R; how to inspect a function, look up its
documentation, and find examples on a function’s use.

*Reading*: [Programming
Basics](https://rstudio.cloud/learn/primers/1.2). *Topics*: `functions`,
`arguments` *Reading Time*: ~ 10 minutes

### **q1** How do you find help on a function? Get help on the built-in `rnorm` function.

``` r
## ?rnorm
```

### **q2** How do you show the source code for a function?

``` r
## rnorm
## rnorm(n, mean = 0, sd = 1)
## .Call(C_rnorm,n, mean, sd
## <bytecode: 0x7fd6458d8b00>
## <environment: namespace:stats>)
```

### **q3** Using either the documentation or the source, determine the arguments for `rnorm`.

n = number of obersvations mean = mean of the normal sd = standrad
dervation of normal

### **q4** Scroll to the bottom of the help for the `library()` function. How do you

list all available packages?

library()

The **examples** in the help documentation are often *extremely* helpful
for learning how to use a function (or reminding yourself how its used)!
Get used to checking the examples, as they’re a great resource.

<!-- include-exit-ticket -->

# Exit Ticket

<!-- -------------------------------------------------- -->

Once you have completed this exercise, make sure to fill out the **exit
ticket survey**, [linked
here](https://docs.google.com/forms/d/e/1FAIpQLSeuq2LFIwWcm05e8-JU84A3irdEL7JkXhMq5Xtoalib36LFHw/viewform?usp=pp_url&entry.693978880=e-setup02-functions-assignment.Rmd).
