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

This dataset `transit_data` contains 19 variables, they are line, station\_name, station\_latitude, station\_longitude, route1, route2, route3, route4, route5, route6, route7, route8, route9, route10, route11, entry, vending, entrance\_type, ada. At first I import the dataset, and clean it's name using the function`janitor::clean_names`, and select required columns, change the variable type of the entry variable to logical factor.

The result dataset is 1868 rows by 19 columns.

These data are still untidy, the route number and route name variables are not clear for people to read.The variables from 'route1' to 'route11' can be gathered, so that the route number and the route name are distinct variables, people can get required information from the dataset more easily.

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

37.704918 percent of station entrances / exits do not have vending allow entrance.

**3. Reformat data.**

``` r
reformat_transit_data = transit_data %>%
  gather(key = route_number, value = route_name, route1:route11)

dis_refo_data = reformat_transit_data %>%
  distinct(station_name, line, .keep_all = TRUE)
```

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
  readxl::read_excel("./data/HealthyHarborWaterWheelTotals2018-7-28.xlsx", sheet = 1, range = cell_cols("A:N")) %>%
  janitor::clean_names() %>%
  na.omit(1) %>%
  mutate(sports_balls = as.integer(sports_balls))
```

**2. Read and clean precipitation data for 2016 and 2017.**

``` r
precipitation_2016 = 
  readxl::read_excel("./data/HealthyHarborWaterWheelTotals2018-7-28.xlsx", sheet = 4,  range = "A2:B14") %>%
  janitor::clean_names() %>%
  na.omit(2) %>%
  mutate(year = "2016")

precipitation_2017 = 
  readxl::read_excel("./data/HealthyHarborWaterWheelTotals2018-7-28.xlsx", sheet = 3, range = "A2:B14") %>%
  janitor::clean_names() %>%
  na.omit(2) %>%
  mutate(year = "2017")

precip_data = 
  bind_rows(precipitation_2016, precipitation_2017) %>%
  mutate(month = (month.name[month])) %>%
  select(year, everything())
```

**3. Write a paragraph about these data.**

The dataset `mr_trash_wheel` has 14 variables, and 285 observations of the dumbsters' trash after cleaning the data. It contains record about dumbster numbers, type of litter and observe time.

The dataset `precipitation_2016` and `precipitation_2017` separately include the precipitation data of 2016 and 2017 in each month. The combined dataset `precip_data` shows the precipitation every month in two years.

**4. What was the total precipitation in 2017?**

The total precipitation in 2017 is 23.5.

**5. What was the median number of sports balls in a dumpster in 2016?**

``` r
mr_trash_wheel_2016 = 
  filter(mr_trash_wheel, year == "2016")
```

The median number of sports balls in a dumbster in 2016 is 26.

Problem 3
=========

**1. Import and tidy data.**

``` r
library(p8105.datasets)

brfss = p8105.datasets::brfss_smart2010 %>%
  filter(Topic == "Overall Health") %>%
  select(-Class, -Topic, -Question, -Sample_Size, -Confidence_limit_Low:-GeoLocation) %>%
  spread(key = Response, value = Data_value) %>%
  janitor::clean_names() %>%
  mutate(good_prop = (excellent + good)/(excellent + good +fair + poor))
```

**2. How many unique locations are included in the dataset? Is every state represented? What state is observed the most? **

``` r
max_freq = as.data.frame(table(brfss$locationabbr))%>%
  filter(Freq == max(Freq)) 
```

There are 404 unique locations in this dataset.

There are 51 states included in this dataset, so every states in United States is represented.

The state "NJ" is observed the most, appeared 146 times.

**3. In 2002, what is the median of the “Excellent” response value? Make a histogram of “Excellent” response values in the year 2002.**

``` r
brfss_2002 = filter(brfss, year == "2002") 

ggplot(brfss_2002, aes(x = excellent))+ geom_histogram(fill = 'pink', alpha = 0.7)
```

![](p8105_homowork2_qs2200_files/figure-markdown_github/problem%203.3-1.png)

In 2002, the median of the "Excellent" response value is 23.6.

**4. Make a scatterplot showing the proportion of “Excellent” response values in New York County and Queens County (both in NY State) in each year from 2002 to 2010.**

``` r
brfss_nycqu = 
  filter(brfss, locationdesc %in% c("NY - New York County", "NY - Queens County")) %>%
  mutate(excellent_prop = excellent/(excellent + poor + fair + good))

ggplot(brfss_nycqu, aes(x = year, y = excellent_prop ))+
  geom_point(aes(color = locationdesc))
```

![](p8105_homowork2_qs2200_files/figure-markdown_github/problem%203.4-1.png)
