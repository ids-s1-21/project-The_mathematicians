Project proposal: students’ beliefs about mathematics
================
The Mathematicians

``` r
library(tidyverse)
library(broom)
library(dplyr)
```

## 1. Introduction

This data analysis is based on the research project of Dr. George
Kinnear (which is also the source from which we’ve obtained our data).
The data is collected from an anonymous survey regarding “Beliefs about
Mathematics” which was completed by students of the University of
Edinburgh for the academic year 2019-2020. Said students belong to the
School of Mathematics (including both students in the School of
Mathematics or those who have the School of Mathematics as their second
school)

Our research aims at investigating the beliefs of students towards
mathematics. There are several sub-questions, which include:  
1. How are answers distributed between students in different schools
(e.g. School of Mathematics vs. School of Informatics)?  
2. Are there any correlations between answers (e.g. whether some
combinations of answers to different questions are more likely to be
chosen simultaneously)?  
3. What are the general attitudes of all students towards mathematics?  
4. How do the answers of students differ from expected answers?

There are 2 raw data sets which need to be processed. Each of them
contains different cases and variables.  
1. responses\_joined (Main data to be analysed)  
The cases are answers to each survey question filled by each
student.There are 22341 cases. The original form of the data contains
677 cases and the answer to each question is given by a separate column.
Therefore it has 37 variables. The original form will also be used
during the analysis process. The reason why the original form is changed
to this form is that we need to satisfy the requirement which limits the
variable number to 10.  
In the process of analysis, there are three major aspects we are
planning to investigate.  
First we are going to consider the correlation between different
questions. For this aspect we will need the `qnum` variable representing
the number of questions and `answer` variable representing the answers
from students for each question. Some questions are in the same category
and measure the same thing, so we are going to group the observational
units based on this `qnum` variable and analyse `answer` in each group.
For example, whether `answer` for some specific `qnum` are more likely
to be the same.  
The second aspect is that we are going to consider the “score” of
students. (i.e. How close their answers are to the expected answer). In
this aspect we need `answer`, `expected_ans`, `qnum` and `anon_id`
variables. We are going to group the answers by `anon_id` which acts as
an identification for each student. Then for answers with certain `qnum`
(excluding the filter question and consent question), we are going to
compare the `answer` (answers from students) to `expected_ans` that
represents the answers from experts.  
For the third aspect, we need to investigate `programme_school_name`,
`answer` and `qnum` variables. We will consider whether there is any
specific pattern for `answer` in certain `programme_school_name`
(representing schools where the student is from). For example, whether
students from some specific schools are more likely to put “Agree” in
their `answer` for some `qnum`. We may also want to investigate whether
there is difference in patterns between `gender`. Other variables may
also be used and for detail description of the data, please refer to the
data dictionary.

## 2. Data

    ## New names:
    ## * ...1 -> ...7

    ## Rows: 22341 Columns: 10

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (8): anon_id, Date, programme_school_name, gender, answer, raw, expected...
    ## dbl (2): qnum, ...7

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## Rows: 22,341
    ## Columns: 10
    ## $ anon_id               <chr> "Sf3f9e6e1", "Sf3f9e6e1", "Sf3f9e6e1", "Sf3f9e6e…
    ## $ Date                  <chr> "Monday, 25 November 2019, 10:56 AM", "Monday, 2…
    ## $ programme_school_name <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ gender                <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ qnum                  <dbl> 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14…
    ## $ answer                <chr> "No, I do not consent", NA, NA, NA, NA, NA, NA, …
    ## $ ...7                  <dbl> NA, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 1…
    ## $ raw                   <chr> NA, "(1) After I study a topic in math and feel …
    ## $ expected_ans          <chr> NA, "Disagree", "Disagree", "Disagree", "Disagre…
    ## $ qtext                 <chr> NA, "After I study a topic in math and feel that…

## 3. Data analysis plan

-   Variables: Multiple outcome (y) and predictor (x) variables are
    needed.  
    For example:  
    Question 1:  
    x should be the question number (qnum excluding 0) and y should be
    the percentage of agrees (i.e. agree/total). The results will also
    be grouped by schools.  
    Question 2:  
    x should be the answers to one group of questions whereas y should
    be the answers to the other groups of questions. (The grouping of
    questions follows the MAPS categories which would be introduced in
    the summary part)  
    Question 3: x should be the question number (qnum excluding 0) and y
    should be the percentage of agrees. (i.e. agree/total)

-   Exploratory data analysis

1.  Number of agrees for each question It’s good to take a quick look at
    how many agrees are there for each question. This could help us
    judge whether there is any pattern for answers to distribute.

``` r
responses_joined_completed %>%
  group_by(qnum) %>%
  summarise(agrees = sum(answer %in% c("Strongly Agree", "Agree"), na.rm = TRUE)) %>%
  arrange(desc(agrees)) %>%
  ggplot() +
  geom_col(mapping = aes(x = factor(qnum), y = agrees)) +
  labs(
    x = "Question No.",
    y = "Number of agrees",
    title = "Number of agrees for each question"
  ) +
  theme_minimal()
```

![](proposal_files/figure-gfm/number-agree-1.png)<!-- -->

2.  Number of students in different schools. By checking the number of
    students in different schools, we are able to get some knowledge on
    the reliability of our results. This is because if a school has a
    very small sample size, then the conclusion about this school
    generated from these samples may not be reliable.

``` r
responses_joined %>%
  group_by(programme_school_name) %>%
  summarise(student_number = n()) %>%
  filter(is.na(programme_school_name) == FALSE) %>%
  arrange(desc(student_number))
```

    ## # A tibble: 16 × 2
    ##    programme_school_name                                  student_number
    ##    <chr>                                                           <int>
    ##  1 School of Informatics                                             254
    ##  2 School of Mathematics                                             149
    ##  3 School of Economics                                                76
    ##  4 School of Physics and Astronomy                                    40
    ##  5 School of Philosophy, Psychology and Language Sciences             26
    ##  6 Business School                                                     4
    ##  7 School of Engineering                                               4
    ##  8 School of Biological Sciences                                       2
    ##  9 School of Chemistry                                                 2
    ## 10 School of Literatures, Languages and Cultures                       2
    ## 11 School of Social and Political Science                              2
    ## 12 College of Arts, Humanities and Social Sciences                     1
    ## 13 Edinburgh College of Art                                            1
    ## 14 Moray House School of Education and Sport                           1
    ## 15 School of Divinity                                                  1
    ## 16 School of Geosciences                                               1

-   Statistical methods:

Our hypothesised result for question 1 is that students in the School of
Mathematics tend to provide answers that are closer to expected answers.
To make a comparisons between these qualitative answers, we need to
transfer qualitative data into quantitative data (change answers like
“Strongly agree”or “Agree” to corresponding marks) and perform
calculations regarding mean, standard deviation, etc… in order to get
the distribution of the data.

Our hypothesised result for question 2 is that there exists a positive
correlation between answers to questions measuring “confidence” and
answers to questions measuring “persistence”. In order to analyse the
correlation between answers, we may need to implement some linearity
test and correlation analysis. These may include using linear regression
models and pearson r correlation test dependent on the situation.
Although these contents have not been covered by this course yet, with
some self-study it’s possible for us to use them in our data analysis.
