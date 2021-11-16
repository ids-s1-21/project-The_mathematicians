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

Although these categories don’t seem to have significant correlation
with others. There are some pairs of categories that have a relatively
higher correlation coefficient than others. For example, the correlation
coefficient between “interest” and “real\_world” is about 0.366 and the
correlation coefficient between “interest” and “sense” is approximately
0.367.
