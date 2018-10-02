p8105\_homework2\_qs2200
================
Qi Shao
10/2/2018

Problem 1
=========

``` r
transit_data = 
  read_csv(file = "./data/NYC_Transit_Subway_Entrance_And_Exit_Data.csv") %>%
  janitor::clean_names() %>%
  select(line, station_name, station_latitude, station_longitude, route1:route11, entry, vending, entrance_type, ada) %>%
  mutate(entry = recode(entry, `YES` = "TRUE", `NO` = "FALSE")) %>%
  mutate(entry = as.logical(entry))
```

**1. Describe the dataset.**

This dataset `transit_data` contains 19 variables, they are line, station\_name, station\_latitude, station\_longitude, route1, route2, route3, route4, route5, route6, route7, route8, route9, route10, route11, entry, vending, entrance\_type, ada. At first I import the dataset, and clean it's name using the function`janitor::clean_names`, and select required columns, change the variable type of the entry variable.

The result dataset is 1868 rows by 19columns.

These data are still untidy, the route number and route name variables are not clear for people to read.

**2. Answer questions using the dataset.**

``` r
dist_transit_data = 
  distinct(transit_data, station_name, line, .keep_all = TRUE)
```

-   How many distinct stations are there?

There are 465 distinct stations in the dataset.

-   How many stations are ADA compliant?

84 stations are ADA compliant.

-   What proportion of station entrances / exits without vending allow entrance?

9.7965739 percent of station entrances / exits do not have vending allow entrance.

**3. Reformat data.**

``` r
reformat_transit_data = transit_data %>%
  gather(key = route_number, value = route_name, route1:route11)

dis_refo_data = reformat_transit_data %>%
  distinct(transit_data, station_name, line, .keep_all = TRUE)
```

    ## Warning: Trying to compute distinct() for variables not found in the data:
    ## - `transit_data`
    ## This is an error, but only a warning is raised for compatibility reasons.
    ## The following variables will be used:
    ## - station_name
    ## - line

-   How many distinct stations serve the A train?

There are 60 distinct stations serve the A train.

-   Of the stations that serve the A train, how many are ADA compliant?

Of all stations that serve the A train, 17 stations are ADA compliant.

Problem 2
=========

**1. Read and clean the Mr. Trash Wheel sheet.**

``` r
library(cellranger)
mr_trash_wheel = 
  readxl::read_excel("./data/HealthyHarborWaterWheelTotals2017-9-26.xlsx", sheet = 1, range = cell_cols("A:N")) %>%
  janitor::clean_names() %>%
  na.omit(1) %>%
  mutate(sports_balls = as.integer(sports_balls))
```
