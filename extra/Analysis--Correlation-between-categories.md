Analysis: Correlations between answers
================
The Mathematicians

## Libraries

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.5     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.0     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(broom)
library(dplyr)
library(patchwork)
library(tidymodels)
```

    ## Registered S3 method overwritten by 'tune':
    ##   method                   from   
    ##   required_pkgs.model_spec parsnip

    ## ── Attaching packages ────────────────────────────────────── tidymodels 0.1.3 ──

    ## ✓ dials        0.0.9      ✓ rsample      0.1.0 
    ## ✓ infer        0.5.4      ✓ tune         0.1.6 
    ## ✓ modeldata    0.1.1      ✓ workflows    0.2.3 
    ## ✓ parsnip      0.1.7      ✓ workflowsets 0.1.0 
    ## ✓ recipes      0.1.16     ✓ yardstick    0.0.8

    ## ── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
    ## x scales::discard() masks purrr::discard()
    ## x dplyr::filter()   masks stats::filter()
    ## x recipes::fixed()  masks stringr::fixed()
    ## x dplyr::lag()      masks stats::lag()
    ## x yardstick::spec() masks readr::spec()
    ## x recipes::step()   masks stats::step()
    ## • Use tidymodels_prefer() to resolve common conflicts.

## Load the necesary data

``` r
responses_numeric1_filtered <- read_csv("responses_numeric1_filtered.csv")
```

    ## Rows: 17696 Columns: 12

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (9): anon_id, Date, programme_school_name, gender, answer, raw, expected...
    ## dbl (3): qnum, ...7, mark

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

## Group the data based on their categories

In order to analyse the correlation between answers to questions in
different categories, we need to firstly group the data based on
categories. And during this section of analysis, we are going to use the
data set where scores are given binominally (either 1 or 0) for each
individual question. Also, we still need to discard those who didn’t
finish the whole survey and just completed a part of the questions (as
they are less likely to do the survey seriously).

``` r
not_finish_list <- responses_numeric1_filtered %>%
  filter(is.na(answer) == TRUE) 
not_finish_list <- as_tibble(unique(not_finish_list$anon_id))
not_finish_list <- not_finish_list %>%
  rename(anon_id = value)

responses_part2 <- anti_join(responses_numeric1_filtered, not_finish_list, by = "anon_id")

responses_summary_part2 <- responses_part2 %>%
  filter(qnum != 19) %>%
  group_by(anon_id, category) %>%
  summarise(sum_category_mark = sum(mark))
```

    ## `summarise()` has grouped output by 'anon_id'. You can override using the `.groups` argument.

``` r
responses_summary_part2 <- responses_summary_part2 %>%
  pivot_wider(names_from = category, values_from = sum_category_mark)
```

## Pearson correlation test

Based on the result we obtained above, we could test the correlations
between different marks for different categories. We are going to use
pearson coefficient which can accurately show their level of
correlation. For those with relative high levels of correlation, we
could do deeper investigations.  
We are going to make a matrix where the intercepted entry between each
row and column represents the correlation between the variables
(category) represented by each row and column (Each row and column will
represent a category)

``` r
correlation_matrix <- data.frame(matrix(ncol = 8, nrow = 8))
rownames(correlation_matrix) <- colnames(responses_summary_part2[2:9])
colnames(correlation_matrix) <- colnames(responses_summary_part2[2:9])
for (i in 1:8) {
  for (j in 1:8) {
    x <- responses_summary_part2[i + 1]
    y <- responses_summary_part2[j + 1]
    correlation_matrix[i, j] <- cor(x, y, method = "pearson")
  }
}

correlation_matrix <- correlation_matrix %>%
  select(-c("no_category"))
correlation_matrix <- correlation_matrix[-c(5),]

print(correlation_matrix)
```

    ##                confidence growth_mindset  interest    nature persistence
    ## confidence      1.0000000     0.10132206 0.3189357 0.1614555  0.31464807
    ## growth_mindset  0.1013221     1.00000000 0.1757021 0.2126943  0.08377009
    ## interest        0.3189357     0.17570212 1.0000000 0.2869885  0.29001460
    ## nature          0.1614555     0.21269430 0.2869885 1.0000000  0.23220735
    ## persistence     0.3146481     0.08377009 0.2900146 0.2322073  1.00000000
    ## real_world      0.0755797     0.24514182 0.3658693 0.2243523  0.16293823
    ## sense           0.1910234     0.22272586 0.3671740 0.2991264  0.33047533
    ##                real_world     sense
    ## confidence      0.0755797 0.1910234
    ## growth_mindset  0.2451418 0.2227259
    ## interest        0.3658693 0.3671740
    ## nature          0.2243523 0.2991264
    ## persistence     0.1629382 0.3304753
    ## real_world      1.0000000 0.3416629
    ## sense           0.3416629 1.0000000

It seems that these categories don’t have significant linear
correlations. Therefore, we may need to fit them to a model other than a
simple linear model. And, after some processings, we can change the
value of the column we need to use to be binomial and then we can apply
logistic regression model.

We know that for each individual question, the mark is either 0 or 1 and
each category contains several questions like this. Therefore, the
distribution of total mark of each category is similar to a binomial
distribution. The difference is that in binomial distribution, the
probability of getting each question correctly is fixed while in this
situation the probability may not be fixed.

However, we know that if a student answers each question by pure
guessing, the probability of answering each question correctly will just
be 2/5 (Since “Neutral” will always get a score of 0). Therefore, we
could calculate a “critical number” such that if a student gets a number
of correct answers that is less than the critical number, than the
probability that the student is guessing the answer is more than the
probability that the student is doing these questions seriously.

Based on this, we could turn the total mark of each student into 1 or 0
according to the following rule: If the number of correct answer is
smaller than the “critical number”, we can mark them as 0, otherwise 1.

When it comes to the calculation of critical number, we could simply
find a number “m” for each category such that the probability for the
number of correct answers to be less than “m” is greater than 50% under
the distribution of Bin(n, 1/2) where n is the total number of question
in each category.

For example, if there are 4 question in a category, we can find that
P(Right answer \<= 2) = (1/2)^4 \* (4C0 + 4C1 + 4C2) = 0.6875 while
P(Right answer \<= 1) \< 0.5. In this case, the “critical number” will
be 2.

According to the method of categorising questions described in the
previous section, we can get the following critical number for each
category:

-   Confidence in Mathematics: 2

-   Persistence in Problem Solving: 2

-   Growth Mindset: 2

-   Interest in Mathematics: 1

-   Relationship between Mathematics and Real World: 1

-   Sense Making: 2

-   Nature of the Answers: 3

First we are going to test our model on “interest” and “real_world”,
where we decided to let “real_world” to be our outcome.

``` r
interest_realworld_data <- responses_summary_part2 %>%
  mutate(
    interest = as_factor(interest),
    real_world = as_factor(if_else(real_world <= 1, 0 ,1)),
  ) %>%
  select(interest, real_world)
```

    ## Adding missing grouping variables: `anon_id`

Before creating a recipe, we first need to split the data into training
data and testing data.

``` r
set.seed(9841)
data_split <- initial_split(interest_realworld_data, prop = 0.8)
training_data = training(data_split)
testing_data = testing(data_split)
```

Then we start to create a data recipe and a corresponding model.

``` r
data_rec <- recipe(
  real_world ~ interest,
  data = training_data
) %>%
  step_dummy(all_nominal(), -all_outcomes())

data_model <- logistic_reg() %>%
  set_engine("glm")
```

After that, we make a workflow and fit the data to the model.

``` r
data_workflow <- workflow() %>%
  add_model(data_model) %>%
  add_recipe(data_rec)

data_fit <- data_workflow %>%
  fit(data = training_data)

tidy(data_fit)
```

    ## # A tibble: 4 × 5
    ##   term        estimate std.error statistic  p.value
    ##   <chr>          <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)   0.0500     0.224     0.224 0.823   
    ## 2 interest_X3   0.972      0.268     3.63  0.000283
    ## 3 interest_X1  -0.561      0.428    -1.31  0.190   
    ## 4 interest_X0  -2.09       0.653    -3.19  0.00140

Then we are going to use this model to predict the outcome of testing
data in order to see whether the model fit is good.

``` r
data_predict <- predict(data_fit, testing_data, type = "prob") %>%
  bind_cols(testing_data)
data_predict %>%
  select(.pred_0, .pred_1, real_world)
```

    ## # A tibble: 94 × 3
    ##    .pred_0 .pred_1 real_world
    ##      <dbl>   <dbl> <fct>     
    ##  1   0.265   0.735 1         
    ##  2   0.265   0.735 1         
    ##  3   0.265   0.735 1         
    ##  4   0.265   0.735 0         
    ##  5   0.265   0.735 1         
    ##  6   0.265   0.735 1         
    ##  7   0.265   0.735 0         
    ##  8   0.265   0.735 1         
    ##  9   0.487   0.513 1         
    ## 10   0.487   0.513 0         
    ## # … with 84 more rows

Also, we can make an roc curve for our model.

``` r
data_predict %>%
  roc_auc(
    truth = real_world,
    .pred_1,
    event_level = "second"
  )
```

    ## # A tibble: 1 × 3
    ##   .metric .estimator .estimate
    ##   <chr>   <chr>          <dbl>
    ## 1 roc_auc binary         0.674

## Writing the model-fitting function (Not finished)

As there are multiple groups of values that is needed to be fitted, we
decided to create a function in order to prevent writing codes
repetitively,

``` r
model_fitting <- function(responses_binary, dep_var, indep_var) {
  class(dep_var)
  #Split the data to training data and testing data
  set.seed(9841)
  data_split <- initial_split(responses_binary, prop = 0.8)
  training_data = training(data_split)
  testing_data = testing(data_split)
  
  #Create a recipe
  data_rec <- recipe(
    dep_var ~ indep_var,
    data = training_data
  ) %>%
    step_dummy(all_nominal(), -all_outcomes())
  data_model <- logistic_reg() %>%
    set_engine("glm")
  
  #Create workflow
  data_workflow <- workflow() %>%
    add_model(data_model) %>%
    add_recipe(data_rec)
  data_fit <- data_workflow %>%
    fit(data = training_data)
  
  #Show the model (to be considered)
  tidy(data_fit)
  return(data_fit)
}
```
