Private and Public Colleges in the US
================

### Loading Data

``` r
library(tidyverse)
```

``` r
colleges <- read.csv("College_Data.csv")
```

### Wrangling for Easier Manipulation and Visualization

``` r
colleges_percent <- colleges %>%
  mutate(percent_acc = Accept/Apps)
```

``` r
cost <- colleges_percent %>%
  mutate(total = Room.Board + Books + Personal) %>%
  filter(!Grad.Rate >= 101) %>%
  mutate(re_private = ifelse(Private == "Yes", "Private", "Public")) %>%
  select(-Private) %>%
  rename(Private = re_private)
```

### Number of Private and Public Colleges in the US

``` r
total_sum <- cost %>%
  group_by(Private) %>%
  summarise(total = n())
knitr::kable(total_sum)
```

| Private |  total|
|:--------|------:|
| Private |    564|
| Public  |    212|

Figure 1: Graduation Rates for Private and Public Colleges
----------------------------------------------------------

``` r
ggplot(cost, aes(x = Grad.Rate)) +
  geom_histogram(fill = "dodgerblue4", bins = 15) +
  facet_wrap(~Private, nrow = 2) +
  labs(x = "Graduation Rate")
```

![](README_files/figure-markdown_github/unnamed-chunk-5-1.png)

Summary Statistics for Graduation Rates
---------------------------------------

``` r
grad_rate_sum <- cost %>%
  group_by(Private) %>%
  summarise(median = median(Grad.Rate),
            min = min(Grad.Rate),
            max = max(Grad.Rate))
knitr::kable(grad_rate_sum)
```

| Private |  median|  min|  max|
|:--------|-------:|----:|----:|
| Private |      69|   15|  100|
| Public  |      55|   10|  100|

Private colleges appear to have higher graduation rates than public colleges. However, the majority of both types of colleges have graduation rates higher than 35-40%.

Figure 2: Room and Board Costs for Private and Public Colleges
--------------------------------------------------------------

``` r
ggplot(cost, aes(x = Room.Board)) +
  geom_histogram(fill = "dodgerblue4", bins = 15) +
  facet_wrap(~Private, nrow = 2) +
  labs(x = "Room and Board Cost")
```

![](README_files/figure-markdown_github/unnamed-chunk-7-1.png)

Summary Statistics for Room and Board Costs
-------------------------------------------

``` r
rb_cost_sum <- cost %>%
  group_by(Private) %>%
  summarise(median = median(Room.Board),
            min = min(Room.Board),
            max = max(Room.Board))
knitr::kable(rb_cost_sum)
```

| Private |  median|   min|   max|
|:--------|-------:|-----:|-----:|
| Private |    4400|  2370|  8124|
| Public  |    3708|  1780|  6540|

Private colleges typically charge more for room and board. However, there is a large amount of overlap between private and public colleges around $3000 to $5000.

Data Source: <https://www.kaggle.com/yashgpt/us-college-data>
