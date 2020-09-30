p8105\_hw2\_vd2392
================
Vihar Desu

### Setup

``` r
library(tidyverse)
```

    ## ── Attaching packages ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.3     ✓ dplyr   1.0.2
    ## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
    ## ✓ readr   1.3.1     ✓ forcats 0.5.0

    ## ── Conflicts ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(ggplot2)
library(readxl)
```

### Problem 1

Here, we read and clean the Mr. Trash Wheel dataset

``` r
trash_df <-
  read_xlsx(
    "./data/Trash-Wheel-Collection-Totals-8-6-19.xlsx",
    sheet = "Mr. Trash Wheel",
    range = cell_cols("A:N")
  ) %>%
  janitor::clean_names() %>%
  drop_na(dumpster) %>%
  mutate(sports_balls = round(sports_balls),
         sports_balls = as.integer(sports_balls))
head(trash_df, 10)
```

    ## # A tibble: 10 x 14
    ##    dumpster month  year date                weight_tons volume_cubic_ya…
    ##       <dbl> <chr> <dbl> <dttm>                    <dbl>            <dbl>
    ##  1        1 May    2014 2014-05-16 00:00:00        4.31               18
    ##  2        2 May    2014 2014-05-16 00:00:00        2.74               13
    ##  3        3 May    2014 2014-05-16 00:00:00        3.45               15
    ##  4        4 May    2014 2014-05-17 00:00:00        3.1                15
    ##  5        5 May    2014 2014-05-17 00:00:00        4.06               18
    ##  6        6 May    2014 2014-05-20 00:00:00        2.71               13
    ##  7        7 May    2014 2014-05-21 00:00:00        1.91                8
    ##  8        8 May    2014 2014-05-28 00:00:00        3.7                16
    ##  9        9 June   2014 2014-06-05 00:00:00        2.52               14
    ## 10       10 June   2014 2014-06-11 00:00:00        3.76               18
    ## # … with 8 more variables: plastic_bottles <dbl>, polystyrene <dbl>,
    ## #   cigarette_butts <dbl>, glass_bottles <dbl>, grocery_bags <dbl>,
    ## #   chip_bags <dbl>, sports_balls <int>, homes_powered <dbl>

### Problem 1 cont.

Next, we read and clean the precipitation datasets for 2017 and 2018

``` r
precip_2018_df <-
  read_xlsx(
    "./data/Trash-Wheel-Collection-Totals-8-6-19.xlsx",
    sheet = "2018 Precipitation",
    skip = 1
  ) %>%
  janitor::clean_names() %>%
  drop_na(month) %>%
  mutate(year = 2018) %>%
  relocate(year)

precip_2017_df <-
  read_xlsx(
    "./data/Trash-Wheel-Collection-Totals-8-6-19.xlsx",
    sheet = "2017 Precipitation",
    skip = 1
  ) %>%
  janitor::clean_names() %>%
  drop_na(month) %>%
  mutate(year = 2017) %>%
  relocate(year)

head(precip)
```

    ##      Mobile      Juneau     Phoenix Little Rock Los Angeles  Sacramento 
    ##        67.0        54.7         7.0        48.5        14.0        17.2

### Problem 1 cont.

Finally, we merge the precipitation datasets using a left join and map
the month column to integers

``` r
month_df = tibble(month = 1:12,
                  month_name = month.name)
precip_df = bind_rows(precip_2017_df, precip_2018_df)
left_join(precip_df, month_df, by = "month")
```

    ## # A tibble: 24 x 4
    ##     year month total month_name
    ##    <dbl> <dbl> <dbl> <chr>     
    ##  1  2017     1  2.34 January   
    ##  2  2017     2  1.46 February  
    ##  3  2017     3  3.57 March     
    ##  4  2017     4  3.99 April     
    ##  5  2017     5  5.64 May       
    ##  6  2017     6  1.4  June      
    ##  7  2017     7  7.09 July      
    ##  8  2017     8  4.44 August    
    ##  9  2017     9  1.95 September 
    ## 10  2017    10  0    October   
    ## # … with 14 more rows

``` r
precip_df
```

    ## # A tibble: 24 x 3
    ##     year month total
    ##    <dbl> <dbl> <dbl>
    ##  1  2017     1  2.34
    ##  2  2017     2  1.46
    ##  3  2017     3  3.57
    ##  4  2017     4  3.99
    ##  5  2017     5  5.64
    ##  6  2017     6  1.4 
    ##  7  2017     7  7.09
    ##  8  2017     8  4.44
    ##  9  2017     9  1.95
    ## 10  2017    10  0   
    ## # … with 14 more rows

### Problem 1 cont.

``` r
sum(precip_2018_df$total, na.rm=TRUE)
```

    ## [1] 70.33

``` r
sports_balls_2017 = 
  filter(trash_df, year == 2017) %>% 
  select(sports_balls)
```

The Mr. Trash Wheels dataset provide an interesting look into the volume
and categories of items that people dispose. The trash dataset contains
344 entries of garbage disposed between the years 2014 and 2019. From
the dataset, we observe that a sum of 6.9367610^{5} tons of garbage has
been collected, sorted and (to the best of it’s ability) categorized
into several trash types like sports balls, plastic bottles, chip bags,
etc. An example of the types of interesting information we can compute
from this dataset include the median number of sports balls collected by
Mr. Trash Wheel in 2017, which is 8. For our precipitation dataset, we
aggregated the precipitation data from 2017 and 2018, where we can see
the amount of precipitation in inches. For 2018, as an example, we
observed a total of 70.33 of precipitation in inches with monthly
measurements. In total, the precipitation dataset contains 24 entries,
one for every month of the 2 years.

### Problem 2
