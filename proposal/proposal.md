Project proposal
================
The mathematician

``` r
library(tidyverse)
library(broom)
library(dplyr)
```

*For instructions on what each section should include, please see the
[project page](https://idsed.digital/assessments/project/#proposal) on
the course website. Remove this text when completing your proposal*.

## 1. Introduction

Our research aims at investigating the performance of elder athletes
(athletes with age greater than 55) in multiple aspects: 1. Whether
there are more female or male athletes with ages above 55? 2. What types
of sports and events they are more likely to participate? 3. Within
athletes with ages above 55, is there any difference between the overall
performance of male and female athletes? Which performs better? etc.

The data is obtained from TidyTuesday. The data is collected using an
observational study on historical records of athletes who participated
Olympics games. Cases are the participation of the athlete. So one
athlete may occupies multiple columns if he/she participated multiple
Olympics games or participated in multiple events in one Olympics games.
There are 1029 cases in total. There are 15 variables. They are: - `ID`:
Unique ID for each athlete. - `Name`: The name of the athlete. - `Sex`:
Sex of the athelete. “F” means female and “M” means male. - `Age`: Age
of the athlete. In `athlete_events.csv`, this variable is greater than
55. - `Height`: Height of the athlete in centimeter. - `Weight`: Weight
of the athlete in kilogram. - `Team`: The sport team of which the
athlete is a member. - `NOC`: IOC country code of the country
represented by the athlete. - `Games`: The Olympics game that the
athlete participated. One athlete could participate multiple games. -
`Year`: The year of the event. - `Season`: The season of the event
(Spring, Summer, Autumn or Winter). - `City`: The city where the
Olympics game was hosted. - `Sport`: The type of sport that the athlete
participated. - `Event`: The event where the athlete took part. -
`Medel`: The medal obtained by the athlete in the event. “NA” indicates
no medal.

## 2. Data

    ## Rows: 1029 Columns: 15

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (10): Name, Sex, Team, NOC, Games, Season, City, Sport, Event, Medal
    ## dbl  (5): ID, Age, Height, Weight, Year

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

    ## Rows: 1,029
    ## Columns: 15
    ## $ ID     <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, …
    ## $ Name   <chr> "Hans Aasns", "Olof Ahlberg", "Emilio lava Sautu", "Antoine Luc…
    ## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M"…
    ## $ Age    <dbl> 57, 71, 63, 57, 58, 60, 75, 65, 65, 56, 60, 64, 64, 64, 64, 68,…
    ## $ Height <dbl> 194, NA, NA, NA, NA, 166, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    ## $ Weight <dbl> 93, NA, NA, NA, NA, 63, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ Team   <chr> "Norway", "Sweden", "Spain", "France", "Philippines", "Belize",…
    ## $ NOC    <chr> "NOR", "SWE", "ESP", "FRA", "PHI", "BIZ", "USA", "AUT", "AUT", …
    ## $ Games  <chr> "1960 Summer", "1948 Summer", "1952 Summer", "1900 Summer", "19…
    ## $ Year   <dbl> 1960, 1948, 1952, 1900, 1992, 1968, 1932, 1936, 1936, 1928, 193…
    ## $ Season <chr> "Summer", "Summer", "Summer", "Summer", "Summer", "Summer", "Su…
    ## $ City   <chr> "Roma", "London", "Helsinki", "Paris", "Barcelona", "Mexico Cit…
    ## $ Sport  <chr> "Shooting", "Art Competitions", "Shooting", "Fencing", "Sailing…
    ## $ Event  <chr> "Shooting Men's Trap", "Art Competitions Mixed Painting, Unknow…
    ## $ Medal  <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, "Gold", NA, NA, NA, NA, NA,…

## 3. Data analysis plan
