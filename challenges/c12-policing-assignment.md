Massachusetts Highway Stops
================
Leslie Bostwick
2025-04-27

- [Grading Rubric](#grading-rubric)
  - [Individual](#individual)
  - [Submission](#submission)
- [Setup](#setup)
  - [**q1** Go to the Stanford Open Policing Project page and download
    the Massachusetts State Police records in `Rds` format. Move the
    data to your `data` folder and match the `filename` to load the
    data.](#q1-go-to-the-stanford-open-policing-project-page-and-download-the-massachusetts-state-police-records-in-rds-format-move-the-data-to-your-data-folder-and-match-the-filename-to-load-the-data)
- [EDA](#eda)
  - [**q2** Do your “first checks” on the dataset. What are the basic
    facts about this
    dataset?](#q2-do-your-first-checks-on-the-dataset-what-are-the-basic-facts-about-this-dataset)
  - [**q3** Check the set of factor levels for `subject_race` and
    `raw_Race`. What do you note about overlap / difference between the
    two
    sets?](#q3-check-the-set-of-factor-levels-for-subject_race-and-raw_race-what-do-you-note-about-overlap--difference-between-the-two-sets)
  - [**q4** Check whether `subject_race` and `raw_Race` match for a
    large fraction of cases. Which of the two hypotheses above is most
    likely, based on your
    results?](#q4-check-whether-subject_race-and-raw_race-match-for-a-large-fraction-of-cases-which-of-the-two-hypotheses-above-is-most-likely-based-on-your-results)
  - [Vis](#vis)
    - [**q5** Compare the *arrest rate*—the fraction of total cases in
      which the subject was arrested—across different factors. Create as
      many visuals (or tables) as you need, but make sure to check the
      trends across all of the `subject` variables. Answer the questions
      under *observations*
      below.](#q5-compare-the-arrest-ratethe-fraction-of-total-cases-in-which-the-subject-was-arrestedacross-different-factors-create-as-many-visuals-or-tables-as-you-need-but-make-sure-to-check-the-trends-across-all-of-the-subject-variables-answer-the-questions-under-observations-below)
- [Modeling](#modeling)
  - [**q6** Run the following code and interpret the regression
    coefficients. Answer the the questions under *observations*
    below.](#q6-run-the-following-code-and-interpret-the-regression-coefficients-answer-the-the-questions-under-observations-below)
  - [**q7** Re-fit the logistic regression from q6 setting `"white"` as
    the reference level for `subject_race`. Interpret the the model
    terms and answer the questions
    below.](#q7-re-fit-the-logistic-regression-from-q6-setting-white-as-the-reference-level-for-subject_race-interpret-the-the-model-terms-and-answer-the-questions-below)
  - [**q8** Re-fit the model using a factor indicating the presence of
    contraband in the subject’s vehicle. Answer the questions under
    *observations*
    below.](#q8-re-fit-the-model-using-a-factor-indicating-the-presence-of-contraband-in-the-subjects-vehicle-answer-the-questions-under-observations-below)
  - [**q9** Go deeper: Pose at least one more question about the data
    and fit at least one more model in support of answering that
    question.](#q9-go-deeper-pose-at-least-one-more-question-about-the-data-and-fit-at-least-one-more-model-in-support-of-answering-that-question)
  - [Further Reading](#further-reading)

*Purpose*: In this last challenge we’ll focus on using logistic
regression to study a large, complicated dataset. Interpreting the
results of a model can be challenging—both in terms of the statistics
and the real-world reasoning—so we’ll get some practice in this
challenge.

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category    | Needs Improvement                                                                                                | Satisfactory                                                                                                               |
|-------------|------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| Effort      | Some task **q**’s left unattempted                                                                               | All task **q**’s attempted                                                                                                 |
| Observed    | Did not document observations, or observations incorrect                                                         | Documented correct observations based on analysis                                                                          |
| Supported   | Some observations not clearly supported by analysis                                                              | All observations clearly supported by analysis (table, graph, etc.)                                                        |
| Assessed    | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support      |
| Specified   | Uses the phrase “more data are necessary” without clarification                                                  | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability                                 | Code sufficiently close to the [style guide](https://style.tidyverse.org/)                                                 |

## Submission

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

*Background*: We’ll study data from the [Stanford Open Policing
Project](https://openpolicing.stanford.edu/data/), specifically their
dataset on Massachusetts State Patrol police stops.

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

``` r
library(broom)
```

    ## Warning: package 'broom' was built under R version 4.4.2

# Setup

<!-- -------------------------------------------------- -->

### **q1** Go to the [Stanford Open Policing Project](https://openpolicing.stanford.edu/data/) page and download the Massachusetts State Police records in `Rds` format. Move the data to your `data` folder and match the `filename` to load the data.

*Note*: An `Rds` file is an R-specific file format. The function
`readRDS` will read these files.

``` r
## TODO: Download the data, move to your data folder, and load it
filename <- "./data/yg821jf8611_ma_statewide_2020_04_01.rds"
df_data <- readRDS(filename)
```

# EDA

<!-- -------------------------------------------------- -->

### **q2** Do your “first checks” on the dataset. What are the basic facts about this dataset?

``` r
head(df_data)
```

    ## # A tibble: 6 × 24
    ##   raw_row_number date       location      county_name   subject_age subject_race
    ##   <chr>          <date>     <chr>         <chr>               <int> <fct>       
    ## 1 1              2007-06-06 MIDDLEBOROUGH Plymouth Cou…          33 white       
    ## 2 2              2007-06-07 SEEKONK       Bristol Coun…          36 white       
    ## 3 3              2007-06-07 MEDFORD       Middlesex Co…          56 white       
    ## 4 4              2007-06-07 MEDFORD       Middlesex Co…          37 white       
    ## 5 5              2007-06-07 EVERETT       Middlesex Co…          22 hispanic    
    ## 6 6              2007-06-07 MEDFORD       Middlesex Co…          34 white       
    ## # ℹ 18 more variables: subject_sex <fct>, type <fct>, arrest_made <lgl>,
    ## #   citation_issued <lgl>, warning_issued <lgl>, outcome <fct>,
    ## #   contraband_found <lgl>, contraband_drugs <lgl>, contraband_weapons <lgl>,
    ## #   contraband_alcohol <lgl>, contraband_other <lgl>, frisk_performed <lgl>,
    ## #   search_conducted <lgl>, search_basis <fct>, reason_for_stop <chr>,
    ## #   vehicle_type <chr>, vehicle_registration_state <fct>, raw_Race <chr>

``` r
summary(df_data)
```

    ##  raw_row_number          date              location         county_name       
    ##  Length:3416238     Min.   :2007-01-01   Length:3416238     Length:3416238    
    ##  Class :character   1st Qu.:2009-04-22   Class :character   Class :character  
    ##  Mode  :character   Median :2011-07-08   Mode  :character   Mode  :character  
    ##                     Mean   :2011-07-16                                        
    ##                     3rd Qu.:2013-08-27                                        
    ##                     Max.   :2015-12-31                                        
    ##                                                                               
    ##   subject_age                     subject_race     subject_sex     
    ##  Min.   :10.00    asian/pacific islander: 166842   male  :2362238  
    ##  1st Qu.:25.00    black                 : 351610   female:1038377  
    ##  Median :34.00    hispanic              : 338317   NA's  :  15623  
    ##  Mean   :36.47    white                 :2529780                   
    ##  3rd Qu.:46.00    other                 :  11008                   
    ##  Max.   :94.00    unknown               :  17017                   
    ##  NA's   :158006   NA's                  :   1664                   
    ##          type         arrest_made     citation_issued warning_issued 
    ##  pedestrian:      0   Mode :logical   Mode :logical   Mode :logical  
    ##  vehicular :3416238   FALSE:3323303   FALSE:1244039   FALSE:2269244  
    ##                       TRUE :92019     TRUE :2171283   TRUE :1146078  
    ##                       NA's :916       NA's :916       NA's :916      
    ##                                                                      
    ##                                                                      
    ##                                                                      
    ##      outcome        contraband_found contraband_drugs contraband_weapons
    ##  warning :1146078   Mode :logical    Mode :logical    Mode :logical     
    ##  citation:2171283   FALSE:28256      FALSE:36296      FALSE:53237       
    ##  summons :      0   TRUE :27474      TRUE :19434      TRUE :2493        
    ##  arrest  :  92019   NA's :3360508    NA's :3360508    NA's :3360508     
    ##  NA's    :   6858                                                       
    ##                                                                         
    ##                                                                         
    ##  contraband_alcohol contraband_other frisk_performed search_conducted
    ##  Mode :logical      Mode :logical    Mode :logical   Mode :logical   
    ##  FALSE:3400070      FALSE:51708      FALSE:51029     FALSE:3360508   
    ##  TRUE :16168        TRUE :4022       TRUE :3602      TRUE :55730     
    ##                     NA's :3360508    NA's :3361607                   
    ##                                                                      
    ##                                                                      
    ##                                                                      
    ##          search_basis     reason_for_stop    vehicle_type      
    ##  k9            :      0   Length:3416238     Length:3416238    
    ##  plain view    :      0   Class :character   Class :character  
    ##  consent       :   6903   Mode  :character   Mode  :character  
    ##  probable cause:  25898                                        
    ##  other         :  18228                                        
    ##  NA's          :3365209                                        
    ##                                                                
    ##  vehicle_registration_state   raw_Race        
    ##  MA     :3053713            Length:3416238    
    ##  CT     :  82906            Class :character  
    ##  NY     :  69059            Mode  :character  
    ##  NH     :  51514                              
    ##  RI     :  39375                              
    ##  (Other): 109857                              
    ##  NA's   :   9814

**Observations**:

- What are the basic facts about this dataset?
- During the data collection of this dataset
  - In the state of MA, the number of MA vehicles pull over was 3053713
  - There were 55830 searchs conducted
  - There were 3602 frisk performed
  - This data was collected from 1-1-2007 to 12-31-2015
- …

Note that we have both a `subject_race` and `race_Raw` column. There are
a few possibilities as to what `race_Raw` represents:

- `race_Raw` could be the race of the police officer in the stop
- `race_Raw` could be an unprocessed version of `subject_race`

Let’s try to distinguish between these two possibilities.

### **q3** Check the set of factor levels for `subject_race` and `raw_Race`. What do you note about overlap / difference between the two sets?

``` r
## TODO: Determine the factor levels for subject_race and raw_Race
level_subject <-
df_data %>% 
  summarise(unique(df_data$subject_race))
```

    ## Warning: Returning more (or less) than 1 row per `summarise()` group was deprecated in
    ## dplyr 1.1.0.
    ## ℹ Please use `reframe()` instead.
    ## ℹ When switching from `summarise()` to `reframe()`, remember that `reframe()`
    ##   always returns an ungrouped data frame and adjust accordingly.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

``` r
level_raw <-
df_data %>% 
  summarise(unique(df_data$raw_Race))
```

    ## Warning: Returning more (or less) than 1 row per `summarise()` group was deprecated in
    ## dplyr 1.1.0.
    ## ℹ Please use `reframe()` instead.
    ## ℹ When switching from `summarise()` to `reframe()`, remember that `reframe()`
    ##   always returns an ungrouped data frame and adjust accordingly.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

``` r
level_subject
```

    ## # A tibble: 7 × 1
    ##   `unique(df_data$subject_race)`
    ##   <fct>                         
    ## 1 white                         
    ## 2 hispanic                      
    ## 3 black                         
    ## 4 asian/pacific islander        
    ## 5 other                         
    ## 6 <NA>                          
    ## 7 unknown

``` r
level_raw
```

    ## # A tibble: 9 × 1
    ##   `unique(df_data$raw_Race)`                   
    ##   <chr>                                        
    ## 1 White                                        
    ## 2 Hispanic                                     
    ## 3 Black                                        
    ## 4 Asian or Pacific Islander                    
    ## 5 Middle Eastern or East Indian (South Asian)  
    ## 6 American Indian or Alaskan Native            
    ## 7 <NA>                                         
    ## 8 None - for no operator present citations only
    ## 9 A

**Observations**:

- What are the unique values for `subject_race`?
  - asian/pacific islander
  - black
  - hispanic
  - white
  - other
  - unknown
- What are the unique values for `raw_Race`?
  - A
  - American Indian or Alaskan Native
  - Asian or Pacific Islander
  - Black
  - Hispanic
  - Middle Eastern or East Indian (South Asian)
  - None - for no operator present citations only
  - White
- What is the overlap between the two sets?
  - Black
  - Hispanic
  - White
  - Asian or Pacific Islanders
- What is the difference between the two sets?
  - raw_race uses terms for admin purposes and non descriptiveness
    abbreviations like “A”
  - raw_race includes specific labels while subject_race is more
    standardized with a consistent format

### **q4** Check whether `subject_race` and `raw_Race` match for a large fraction of cases. Which of the two hypotheses above is most likely, based on your results?

*Note*: Just to be clear, I’m *not* asking you to do a *statistical*
hypothesis test.

``` r
## TODO: Devise your own way to test the hypothesis posed above.

df_data <-
  df_data %>% 
  mutate(
    raw_Race_rename = case_when(
      raw_Race %in% c("White") ~ "white",
      raw_Race %in% c("Black") ~ "black",
      raw_Race %in% c("Hispanic") ~ "hispanic",
      raw_Race %in% c("Asian or Pacific Islander") ~ "asian/pacific islander",
      raw_Race %in% c("American Indian or Alaskan Native", 
                      "Middle Eastern or East Indian (South Asian)") ~ "other",
      raw_Race %in% c("None - for no operator present citations only", "A") ~ "unknown",
      TRUE ~ NA_character_
    )
  )

race_match_result <-
  df_data %>% 
  mutate(race_match = subject_race == raw_Race_rename) %>% 
  count(race_match) %>% 
  mutate(percentage = n / sum(n) * 100)

race_match_result
```

    ## # A tibble: 3 × 3
    ##   race_match       n percentage
    ##   <lgl>        <int>      <dbl>
    ## 1 FALSE        64552     1.89  
    ## 2 TRUE       3350022    98.1   
    ## 3 NA            1664     0.0487

**Observations**

Between the two hypotheses:

- `race_Raw` could be the race of the police officer in the stop
- `race_Raw` could be an unprocessed version of `subject_race`

which is most plausible, based on your results?

- I think race_Raw is a unprocessed version of Subject_race as they have
  a 98.06% match to one another

## Vis

<!-- ------------------------- -->

### **q5** Compare the *arrest rate*—the fraction of total cases in which the subject was arrested—across different factors. Create as many visuals (or tables) as you need, but make sure to check the trends across all of the `subject` variables. Answer the questions under *observations* below.

(Note: Create as many chunks and visuals as you need)

``` r
df_q5 <-
  df_data %>% 
  mutate(arrest_made = ifelse(is.na(arrest_made), FALSE, arrest_made))

df_q5 %>% 
  filter(!is.na(subject_age)) %>% 
  group_by(subject_age) %>% 
  summarize(arrest_rate = mean(arrest_made)) %>% 
  ggplot(aes(x = subject_age, y = arrest_rate)) +
  geom_col() +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))
```

![](c12-policing-assignment_files/figure-gfm/q5%201-1.png)<!-- -->

``` r
df_q5 %>% 
  filter(!is.na(subject_sex)) %>% 
  group_by(subject_sex) %>% 
  summarise(arrest_rate = mean(arrest_made)) %>% 
  ggplot(aes(x = subject_sex, y = arrest_rate, fill = subject_sex)) +
  geom_col() 
```

![](c12-policing-assignment_files/figure-gfm/q5%202-1.png)<!-- -->

``` r
df_q5 %>% 
  filter(!is.na(subject_race)) %>% 
  group_by(subject_race) %>% 
  summarise(arrest_rate = mean(arrest_made)) %>% 
  ggplot(aes(x = subject_race, y = arrest_rate, fill = subject_race)) +
  geom_col()
```

![](c12-policing-assignment_files/figure-gfm/q5%203-1.png)<!-- -->

**Observations**:

- How does `arrest_rate` tend to vary with `subject_age`?
  - A lot of 15 year olds are getting arrested at a higher rate than any
    other age. This might be because you have to be 16 to get a
    learner’s permit in MA. The peak of arrest_rate is at 15 years and
    generally, the older you are, the lower the arrest rate is.
- How does `arrest_rate` tend to vary with `subject_sex`?
  - Gerernally, the arrest rate is higher for males than females
- How does `arrest_rate` tend to vary with `subject_race`?
  - Hispanic have the highest arrest rate, then black, then other, then
    white and finally asian/pacific Islander

# Modeling

<!-- -------------------------------------------------- -->

We’re going to use a model to study the relationship between `subject`
factors and arrest rate, but first we need to understand a bit more
about *dummy variables*

### **q6** Run the following code and interpret the regression coefficients. Answer the the questions under *observations* below.

``` r
## NOTE: No need to edit; inspect the estimated model terms.
fit_q6 <-
  glm(
    formula = arrest_made ~ subject_age + subject_race + subject_sex,
    data = df_data %>%
      filter(
        !is.na(arrest_made),
        subject_race %in% c("white", "black", "hispanic")
      ),
    family = "binomial"
  )

fit_q6 %>% tidy()
```

    ## # A tibble: 5 × 5
    ##   term                 estimate std.error statistic   p.value
    ##   <chr>                   <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)           -2.67    0.0132      -202.  0        
    ## 2 subject_age           -0.0142  0.000280     -50.5 0        
    ## 3 subject_racehispanic   0.513   0.0119        43.3 0        
    ## 4 subject_racewhite     -0.380   0.0103       -37.0 3.12e-299
    ## 5 subject_sexfemale     -0.755   0.00910      -83.0 0

**Observations**:

- Which `subject_race` levels are included in fitting the model?
  - The model includes white, black, and Hispanic individuals
- Which `subject_race` levels have terms in the model?
  - The terms only list out whites and hispanics, suggesting that black
    individuals are being used as the reference.

You should find that each factor in the model has a level *missing* in
its set of terms. This is because R represents factors against a
*reference level*: The model treats one factor level as “default”, and
each factor model term represents a change from that “default” behavior.
For instance, the model above treats `subject_sex==male` as the
reference level, so the `subject_sexfemale` term represents the *change
in probability* of arrest due to a person being female (rather than
male).

The this reference level approach to coding factors is necessary for
[technical
reasons](https://www.andrew.cmu.edu/user/achoulde/94842/lectures/lecture10/lecture10-94842.html#why-is-one-of-the-levels-missing-in-the-regression),
but it complicates interpreting the model results. For instance; if we
want to compare two levels, neither of which are the reference level, we
have to consider the difference in their model coefficients. But if we
want to compare all levels against one “baseline” level, then we can
relevel the data to facilitate this comparison.

By default `glm` uses the first factor level present as the reference
level. Therefore we can use
`mutate(factor = fct_relevel(factor, "desired_level"))` to set our
`"desired_level"` as the reference factor.

### **q7** Re-fit the logistic regression from q6 setting `"white"` as the reference level for `subject_race`. Interpret the the model terms and answer the questions below.

``` r
## TODO: Re-fit the logistic regression, but set "white" as the reference
## level for subject_race
 
fit_q7 <-
df_data %>% 
filter(
    !is.na(arrest_made),
    subject_race %in% c("white", "black", "hispanic")
) %>%   
mutate(
    subject_race = fct_relevel(subject_race, "white")
) %>% 
glm(
    formula = arrest_made ~ subject_age + subject_race + subject_sex,
    family = "binomial"
  )

fit_q7 %>% tidy()
```

    ## # A tibble: 5 × 5
    ##   term                 estimate std.error statistic   p.value
    ##   <chr>                   <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)           -3.05    0.0109      -279.  0        
    ## 2 subject_age           -0.0142  0.000280     -50.5 0        
    ## 3 subject_raceblack      0.380   0.0103        37.0 3.12e-299
    ## 4 subject_racehispanic   0.893   0.00859      104.  0        
    ## 5 subject_sexfemale     -0.755   0.00910      -83.0 0

**Observations**:

- Which `subject_race` level has the highest probability of being
  arrested, according to this model? Which has the lowest probability?
  - Hispanic individuals are the most likely highest probabilty followed
    by black individual and then with the lowest probability white
    individuals.
- What could explain this difference in probabilities of arrest across
  race? List **multiple** possibilities.
  - Geography
  - Policing bias
  - The political climate at the of being pulled over
- Look at the set of variables in the dataset; do any of the columns
  relate to a potential explanation you listed?
  - There is a contraband_found variables and recreational weed was not
    legal back when this data set was being collected unlike present day
    in MA so police could have been seeking it out.

One way we can explain differential arrest rates is to include some
measure indicating the presence of an arrestable offense. We’ll do this
in a particular way in the next task.

### **q8** Re-fit the model using a factor indicating the presence of contraband in the subject’s vehicle. Answer the questions under *observations* below.

``` r
## TODO: Repeat the modeling above, but control for whether contraband was found
## during the police stop
fit_q8 <-
df_data %>% 
filter(
    !is.na(arrest_made),
    subject_race %in% c("white", "black", "hispanic")
) %>%   
mutate(
    subject_race = fct_relevel(subject_race, "white"),
    contraband_found = ifelse(is.na(contraband_found), FALSE, contraband_found)
) %>% 
glm(
    formula = arrest_made ~ subject_age + subject_race + subject_sex + contraband_found,
    family = "binomial"
  )

fit_q8 %>% tidy()
```

    ## # A tibble: 6 × 5
    ##   term                 estimate std.error statistic   p.value
    ##   <chr>                   <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)           -3.31    0.0113      -293.  0        
    ## 2 subject_age           -0.0101  0.000284     -35.4 1.34e-274
    ## 3 subject_raceblack      0.351   0.0105        33.3 1.14e-242
    ## 4 subject_racehispanic   0.880   0.00885       99.5 0        
    ## 5 subject_sexfemale     -0.694   0.00921      -75.4 0        
    ## 6 contraband_foundTRUE   3.02    0.0135       223.  0

**Observations**:

- How does controlling for found contraband affect the `subject_race`
  terms in the model?
  - It dropped both estimate by ~.02 for hispanic & black individuals.
- What does the *finding of contraband* tell us about the stop? What
  does it *not* tell us about the stop?
  - It tells us that a search occurred that was likely to lead to an
    arrest.
  - It does not tell us why the police decided to conduct a search.

### **q9** Go deeper: Pose at least one more question about the data and fit at least one more model in support of answering that question.

Are racial disparities in arrest driven significantly by if searchs are
conducted rather than difference in actual behavior or contraband
possession?

``` r
fit_q9 <-
  df_data %>% 
    filter(
     !is.na(search_conducted),
     subject_race %in% c("white","black","hispanic")
    ) %>% 
   mutate(
     subject_race = fct_relevel(subject_race, "white"),
     search_conducted = ifelse(is.na(search_conducted), FALSE, search_conducted)
   ) %>% 
   glm(
     formula = search_conducted ~ subject_age + subject_race + subject_sex,
     family = "binomial"
    )

fit_q9 %>% 
  tidy()
```

    ## # A tibble: 5 × 5
    ##   term                 estimate std.error statistic   p.value
    ##   <chr>                   <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)           -2.76    0.0141      -195.  0        
    ## 2 subject_age           -0.0379  0.000406     -93.2 0        
    ## 3 subject_raceblack      0.460   0.0124        37.1 2.50e-301
    ## 4 subject_racehispanic   0.685   0.0113        60.8 0        
    ## 5 subject_sexfemale     -0.729   0.0114       -63.8 0

**Observations**:

- Hispanic individuals are more likely to get searched versus their
  black counterparts
- Female are less likely to get searched
- Younger people are slight more likely to be search than older people
- Personal police bias against hispanics could be a reason why they’re
  more likely to get search especially in today’s political climate.

## Further Reading

<!-- -------------------------------------------------- -->

- Stanford Open Policing Project
  [findings](https://openpolicing.stanford.edu/findings/).
