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
