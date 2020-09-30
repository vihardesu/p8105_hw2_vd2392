p8105\_hw2\_vd2392
================
Vihar Desu

## Problem 1

Here, we read and clean the Mr. Trash Wheel dataset.

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
```

### Trash Wheel Collection Totals (8-6-19)

| dumpster | month | year | date       | weight\_tons | volume\_cubic\_yards | plastic\_bottles | polystyrene | cigarette\_butts | glass\_bottles | grocery\_bags | chip\_bags | sports\_balls | homes\_powered |
| -------: | :---- | ---: | :--------- | -----------: | -------------------: | ---------------: | ----------: | ---------------: | -------------: | ------------: | ---------: | ------------: | -------------: |
|        1 | May   | 2014 | 2014-05-16 |         4.31 |                   18 |             1450 |        1820 |           126000 |             72 |           584 |       1162 |             7 |              0 |
|        2 | May   | 2014 | 2014-05-16 |         2.74 |                   13 |             1120 |        1030 |            91000 |             42 |           496 |        874 |             5 |              0 |
|        3 | May   | 2014 | 2014-05-16 |         3.45 |                   15 |             2450 |        3100 |           105000 |             50 |          1080 |       2032 |             6 |              0 |
|        4 | May   | 2014 | 2014-05-17 |         3.10 |                   15 |             2380 |        2730 |           100000 |             52 |           896 |       1971 |             6 |              0 |
|        5 | May   | 2014 | 2014-05-17 |         4.06 |                   18 |              980 |         870 |           120000 |             72 |           368 |        753 |             7 |              0 |
|        6 | May   | 2014 | 2014-05-20 |         2.71 |                   13 |             1430 |        2140 |            90000 |             46 |           672 |       1144 |             5 |              0 |

Trash Wheel Collection Totals (8-6-19)

Next, we read and clean the precipitation datasets for 2017 and 2018.

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
```

Finally, we merge the precipitation datasets using a left join and map
the month column to integers.

``` r
month_df = tibble(month = 1:12,
                  month_name = month.name)
precip_df = bind_rows(precip_2017_df, precip_2018_df)
left_join(precip_df, month_df, by = "month")
```

<div data-pagedtable="false">

<script data-pagedtable-source type="application/json">
{"columns":[{"label":["year"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["month"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["total"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["month_name"],"name":[4],"type":["chr"],"align":["left"]}],"data":[{"1":"2017","2":"1","3":"2.34","4":"January"},{"1":"2017","2":"2","3":"1.46","4":"February"},{"1":"2017","2":"3","3":"3.57","4":"March"},{"1":"2017","2":"4","3":"3.99","4":"April"},{"1":"2017","2":"5","3":"5.64","4":"May"},{"1":"2017","2":"6","3":"1.40","4":"June"},{"1":"2017","2":"7","3":"7.09","4":"July"},{"1":"2017","2":"8","3":"4.44","4":"August"},{"1":"2017","2":"9","3":"1.95","4":"September"},{"1":"2017","2":"10","3":"0.00","4":"October"},{"1":"2017","2":"11","3":"0.11","4":"November"},{"1":"2017","2":"12","3":"0.94","4":"December"},{"1":"2018","2":"1","3":"0.94","4":"January"},{"1":"2018","2":"2","3":"4.80","4":"February"},{"1":"2018","2":"3","3":"2.69","4":"March"},{"1":"2018","2":"4","3":"4.69","4":"April"},{"1":"2018","2":"5","3":"9.27","4":"May"},{"1":"2018","2":"6","3":"4.77","4":"June"},{"1":"2018","2":"7","3":"10.20","4":"July"},{"1":"2018","2":"8","3":"6.45","4":"August"},{"1":"2018","2":"9","3":"10.47","4":"September"},{"1":"2018","2":"10","3":"2.12","4":"October"},{"1":"2018","2":"11","3":"7.82","4":"November"},{"1":"2018","2":"12","3":"6.11","4":"December"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>

</div>

### Precipitation Data 2017-2018

| year | month | total |
| ---: | ----: | ----: |
| 2017 |     1 |  2.34 |
| 2017 |     2 |  1.46 |
| 2017 |     3 |  3.57 |
| 2017 |     4 |  3.99 |
| 2017 |     5 |  5.64 |
| 2017 |     6 |  1.40 |

Precipitation Data 2017-2018

#### Problem 1 Summary

The Mr. Trash Wheels dataset provide an interesting look into the volume
and categories of items that people dispose. The trash dataset contains
344 entries of garbage disposed between the years 2014 and 2019. From
the dataset, we observe that a sum of 6.9367610^{5} tons of garbage has
been collected, sorted and (to the best of someone’s ability)
categorized into several trash types like sports balls, plastic bottles,
chip bags, etc. An example of the types of interesting information we
can compute from this dataset include the median number of sports balls
collected by Mr. Trash Wheel in 2017, which is 8. For our precipitation
dataset, we aggregated the precipitation data from 2017 and 2018, where
we can see the amount of precipitation in inches. For 2018, as an
example, we observed a total of 70.33 of precipitation in inches with
monthly measurements. In total, the precipitation dataset contains 24
entries, one for every month of the 2 years.

## Problem 2

``` r
subway_df =
  read_csv(file = "./data/NYC_Transit_Subway_Entrance_And_Exit_Data.csv") %>%
  janitor::clean_names() %>%
  select(line:vending, ada,-exit_only) %>%
  mutate(
    entry = recode(entry, "YES" = 1, "NO" = 0),
    vending = recode(vending, "YES" = 1, "NO" = 0)
  )
```

The NYC Subway Transit dataset contains information about the subway
entry and exit points throughout the city of New York. With over 1868
entry or exit points into the Transit system, this dataset makes it easy
to appreciate just how accessible the city can be. Furthermore, the
dataset covers whether or not a certain point in the subway system is
available for entry or exit, the exact locations of these points, routes
that subway lines take across the city, whether or not they are ADA
compliant, etc. You can do everything from finding directions between
two subway points and creating a geospatial map of subway paths. The
data cleaning process for this data frame involved reading in CSV data,
cleaning the names so that they are R-safe, selecting relevant columns
and mutating some to be R-compatible for further analysis as well. This
dataset is 1868 rows by 19 columns. However, it is not tidy yet because
we observe superfluous columns that can be melted down to be more
succinct with pivot longer call.

``` r
nrow(distinct(subway_df, line, station_name))

ada_compliant_stations =
  distinct(subway_df, line, station_name, .keep_all = TRUE) %>%
  filter(ada == "TRUE")

entrances_and_exits_without_vending = 
  read_csv(file = "./data/NYC_Transit_Subway_Entrance_And_Exit_Data.csv") %>%
  janitor::clean_names() %>%
  filter(exit_only == "Yes", vending == "NO")

entrances_without_vending =
  select(subway_df, everything()) %>%
  filter(vending == 0, entry == 1)

proportion = nrow(entrances_without_vending) / nrow(entrances_and_exits_without_vending)
```

##### Distinct Stations: 465

##### ADA Compliant Stations: 84

##### Station Entrances/Exits Without Vending: 1.38

``` r
subway_df$route8 <- as.character(subway_df$route8)
subway_df$route9 <- as.character(subway_df$route9)
subway_df$route10 <- as.character(subway_df$route10)
subway_df$route11 <- as.character(subway_df$route11)

cleaned_subway_df = 
  pivot_longer(
    subway_df,
    route1:route11,
    names_to = "route_number",
    names_prefix = "route",
    values_to = "route_name"
  )
```

### Subway Entry Data for New York City

| line     | station\_name | station\_latitude | station\_longitude | entrance\_type | entry | vending | ada   | route\_number | route\_name |
| :------- | :------------ | ----------------: | -----------------: | :------------- | ----: | ------: | :---- | :------------ | :---------- |
| 4 Avenue | 25th St       |           40.6604 |         \-73.99809 | Stair          |     1 |       1 | FALSE | 1             | R           |
| 4 Avenue | 25th St       |           40.6604 |         \-73.99809 | Stair          |     1 |       1 | FALSE | 2             | NA          |
| 4 Avenue | 25th St       |           40.6604 |         \-73.99809 | Stair          |     1 |       1 | FALSE | 3             | NA          |
| 4 Avenue | 25th St       |           40.6604 |         \-73.99809 | Stair          |     1 |       1 | FALSE | 4             | NA          |
| 4 Avenue | 25th St       |           40.6604 |         \-73.99809 | Stair          |     1 |       1 | FALSE | 5             | NA          |
| 4 Avenue | 25th St       |           40.6604 |         \-73.99809 | Stair          |     1 |       1 | FALSE | 6             | NA          |

``` r
#Unique Stations Serving A
stations_serving_a =
  nrow(
    cleaned_subway_df %>%
      filter(route_name == "A") %>%
      distinct(line, station_name, .keep_all = TRUE)
  )

ada_compliant_stations_serving_a =
  nrow(
    cleaned_subway_df %>%
      filter(route_name == "A", ada == "TRUE") %>%
      distinct(line, station_name, .keep_all = TRUE)
  )
```

##### Distinct Stations Serving A: 60

##### ADA Compliant Stations Serving A: 17

### Problem 3

``` r
months = array(month.name)

pols_month = read_csv("./data/pols-month.csv") %>% 
  janitor::clean_names() %>% 
  separate(mon, c("year", "month", "day"), "-") %>% 
  mutate (
    month = months[as.numeric(month)],
    prez_gop = as.logical(prez_gop),
    prez_dem = as.logical(prez_dem),
    president = (case_when(prez_gop == TRUE ~ "GOP",
                           prez_dem == TRUE ~ "Democrat"))
  ) %>% 
  select(
    year,
    month,
    president,
    everything(),
    -day,
    -prez_gop,
    -prez_dem
  ) %>% 
  arrange(year, month)
```

### Cleaned Politics by Month Data

| year | month    | president | gov\_gop | sen\_gop | rep\_gop | gov\_dem | sen\_dem | rep\_dem |
| :--- | :------- | :-------- | -------: | -------: | -------: | -------: | -------: | -------: |
| 1947 | April    | Democrat  |       23 |       51 |      253 |       23 |       45 |      198 |
| 1947 | August   | Democrat  |       23 |       51 |      253 |       23 |       45 |      198 |
| 1947 | December | Democrat  |       24 |       51 |      253 |       23 |       45 |      198 |
| 1947 | February | Democrat  |       23 |       51 |      253 |       23 |       45 |      198 |
| 1947 | January  | Democrat  |       23 |       51 |      253 |       23 |       45 |      198 |
| 1947 | July     | Democrat  |       23 |       51 |      253 |       23 |       45 |      198 |

``` r
snp = read_csv("./data/snp.csv") %>% 
  janitor::clean_names() %>% 
  separate(date, c("month", "day", "year"), "/") %>%
  mutate (
    month = months[as.numeric(month)]
  ) %>% 
  select (
    year,
    month,
    close,
    -day
  ) %>% 
  arrange(year, month)
```

### Cleaned S\&P by Month Data

| year | month    | close |
| :--- | :------- | ----: |
| 1950 | April    | 17.96 |
| 1950 | August   | 18.42 |
| 1950 | December | 20.43 |
| 1950 | February | 17.22 |
| 1950 | January  | 17.05 |
| 1950 | July     | 17.84 |

### Cleaned Unemployment by Month Data

| year | month    | unemployment |
| :--- | :------- | -----------: |
| 1948 | April    |          3.9 |
| 1948 | August   |          3.9 |
| 1948 | December |          4.0 |
| 1948 | February |          3.8 |
| 1948 | January  |          3.4 |
| 1948 | July     |          3.6 |

### Tidy, Merged Unemployment, SNP and Pols Data by Months

``` r
merged_data = 
  full_join(
    pols_month,
    snp,
    by = NULL
  ) %>% 
  full_join(
    unemployment,
    by = NULL
  )
```

| year | month    | president | gov\_gop | sen\_gop | rep\_gop | gov\_dem | sen\_dem | rep\_dem | close | unemployment |
| :--- | :------- | :-------- | -------: | -------: | -------: | -------: | -------: | -------: | ----: | -----------: |
| 1947 | April    | Democrat  |       23 |       51 |      253 |       23 |       45 |      198 |    NA |           NA |
| 1947 | August   | Democrat  |       23 |       51 |      253 |       23 |       45 |      198 |    NA |           NA |
| 1947 | December | Democrat  |       24 |       51 |      253 |       23 |       45 |      198 |    NA |           NA |
| 1947 | February | Democrat  |       23 |       51 |      253 |       23 |       45 |      198 |    NA |           NA |
| 1947 | January  | Democrat  |       23 |       51 |      253 |       23 |       45 |      198 |    NA |           NA |
| 1947 | July     | Democrat  |       23 |       51 |      253 |       23 |       45 |      198 |    NA |           NA |

Our final merged dataset is a tidy, cleaned up version of 3 datasets:
pols-month, snp and unemployment data. Our final dataset contains data
from 1947 to 2015 With over 828 entries of data for each month between
those years, we have many useful pieces of information. Our column size
for this dataset is 11, and it contains a combination of three datasets
described below. The pols-month dataset contained the number of
representatives, governors, and senators that were republican and the
equivalent for democrats. We cleaned the data in the first few steps to
cleanly categorize which party held the presidential seat every month
for our time frame. The snp dataset contained useful information
regarding the closing values of the S\&P stock index on the associated
date, which in our case was generalized into that specific month. As you
can see in the merged data, there is a little gap in the information and
they years don’t align perfectly. That is true too for the unemployment
dataset, which contains the unemployment rate in the United States for
various months throughout history.
