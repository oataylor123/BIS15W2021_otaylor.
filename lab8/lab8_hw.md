---
title: "Lab 8 Homework"
author: "Olivia Taylor"
date: "2021-02-18"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  


## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydney_beaches <- readr::read_csv(here("lab8", "data", "sydneybeaches.csv")) %>% janitor::clean_names()
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   BeachId = col_double(),
##   Region = col_character(),
##   Council = col_character(),
##   Site = col_character(),
##   Longitude = col_double(),
##   Latitude = col_double(),
##   Date = col_character(),
##   `Enterococci (cfu/100ml)` = col_double()
## )
```

```r
sydney_beaches
```

```
## # A tibble: 3,690 x 8
##    beach_id region    council   site   longitude latitude date  enterococci_cfu~
##       <dbl> <chr>     <chr>     <chr>      <dbl>    <dbl> <chr>            <dbl>
##  1       25 Sydney C~ Randwick~ Clove~      151.    -33.9 02/0~               19
##  2       25 Sydney C~ Randwick~ Clove~      151.    -33.9 06/0~                3
##  3       25 Sydney C~ Randwick~ Clove~      151.    -33.9 12/0~                2
##  4       25 Sydney C~ Randwick~ Clove~      151.    -33.9 18/0~               13
##  5       25 Sydney C~ Randwick~ Clove~      151.    -33.9 30/0~                8
##  6       25 Sydney C~ Randwick~ Clove~      151.    -33.9 05/0~                7
##  7       25 Sydney C~ Randwick~ Clove~      151.    -33.9 11/0~               11
##  8       25 Sydney C~ Randwick~ Clove~      151.    -33.9 23/0~               97
##  9       25 Sydney C~ Randwick~ Clove~      151.    -33.9 07/0~                3
## 10       25 Sydney C~ Randwick~ Clove~      151.    -33.9 25/0~                0
## # ... with 3,680 more rows
```



2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

Yes, they are tidy; they are in long format and each variable is on its own and each observation is in its own row. 

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`


```r
sydneybeaches_long <- 
  sydney_beaches %>% 
  select(site, date, enterococci_cfu_100ml)
sydneybeaches_long
```

```
## # A tibble: 3,690 x 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # ... with 3,680 more rows
```


4. Pivot the data such that the dates are column names and each beach only appears once. Name the object `sydneybeaches_wide`


```r
sydneybeaches_wide <- 
sydneybeaches_long %>% 
  pivot_wider(names_from = "date",
             values_from = "enterococci_cfu_100ml")
sydneybeaches_wide
```

```
## # A tibble: 11 x 345
##    site  `02/01/2013` `06/01/2013` `12/01/2013` `18/01/2013` `30/01/2013`
##    <chr>        <dbl>        <dbl>        <dbl>        <dbl>        <dbl>
##  1 Clov~           19            3            2           13            8
##  2 Coog~           15            4           17           18           22
##  3 Gord~           NA           NA           NA           NA           NA
##  4 Litt~            9            3           72            1           44
##  5 Mala~            2            4          390           15           13
##  6 Maro~            1            1           20            2           11
##  7 Sout~            1            0           33            2           13
##  8 Sout~           12            2          110           13          100
##  9 Bond~            3            1            2            1            6
## 10 Bron~            4            2           38            3           25
## 11 Tama~            1            0            7           22           23
## # ... with 339 more variables: `05/02/2013` <dbl>, `11/02/2013` <dbl>,
## #   `23/02/2013` <dbl>, `07/03/2013` <dbl>, `25/03/2013` <dbl>,
## #   `02/04/2013` <dbl>, `12/04/2013` <dbl>, `18/04/2013` <dbl>,
## #   `24/04/2013` <dbl>, `01/05/2013` <dbl>, `20/05/2013` <dbl>,
## #   `31/05/2013` <dbl>, `06/06/2013` <dbl>, `12/06/2013` <dbl>,
## #   `24/06/2013` <dbl>, `06/07/2013` <dbl>, `18/07/2013` <dbl>,
## #   `24/07/2013` <dbl>, `08/08/2013` <dbl>, `22/08/2013` <dbl>,
## #   `29/08/2013` <dbl>, `24/01/2013` <dbl>, `17/02/2013` <dbl>,
## #   `01/03/2013` <dbl>, `13/03/2013` <dbl>, `19/03/2013` <dbl>,
## #   `06/04/2013` <dbl>, `07/05/2013` <dbl>, `14/05/2013` <dbl>,
## #   `25/05/2013` <dbl>, `17/06/2013` <dbl>, `30/06/2013` <dbl>,
## #   `12/07/2013` <dbl>, `30/07/2013` <dbl>, `14/08/2013` <dbl>,
## #   `16/08/2013` <dbl>, `02/09/2013` <dbl>, `28/09/2013` <dbl>,
## #   `04/10/2013` <dbl>, `12/10/2013` <dbl>, `28/10/2013` <dbl>,
## #   `05/11/2013` <dbl>, `29/11/2013` <dbl>, `07/12/2013` <dbl>,
## #   `10/09/2013` <dbl>, `16/09/2013` <dbl>, `22/09/2013` <dbl>,
## #   `20/10/2013` <dbl>, `13/11/2013` <dbl>, `21/11/2013` <dbl>,
## #   `23/12/2013` <dbl>, `13/12/2013` <dbl>, `31/12/2013` <dbl>,
## #   `08/10/2014` <dbl>, `16/10/2014` <dbl>, `25/10/2014` <dbl>,
## #   `02/11/2014` <dbl>, `10/11/2014` <dbl>, `18/11/2014` <dbl>,
## #   `13/12/2014` <dbl>, `19/12/2014` <dbl>, `16/01/2014` <dbl>,
## #   `24/01/2014` <dbl>, `09/02/2014` <dbl>, `25/02/2014` <dbl>,
## #   `05/03/2014` <dbl>, `06/04/2014` <dbl>, `06/05/2014` <dbl>,
## #   `16/05/2014` <dbl>, `22/05/2014` <dbl>, `10/06/2014` <dbl>,
## #   `26/06/2014` <dbl>, `08/07/2014` <dbl>, `14/07/2014` <dbl>,
## #   `24/07/2014` <dbl>, `05/08/2014` <dbl>, `21/08/2014` <dbl>,
## #   `21/10/2014` <dbl>, `26/11/2014` <dbl>, `04/12/2014` <dbl>,
## #   `12/09/2014` <dbl>, `18/09/2014` <dbl>, `24/09/2014` <dbl>,
## #   `28/12/2014` <dbl>, `08/01/2014` <dbl>, `01/02/2014` <dbl>,
## #   `17/02/2014` <dbl>, `13/03/2014` <dbl>, `21/03/2014` <dbl>,
## #   `29/03/2014` <dbl>, `22/04/2014` <dbl>, `14/04/2014` <dbl>,
## #   `30/04/2014` <dbl>, `12/05/2014` <dbl>, `28/05/2014` <dbl>,
## #   `03/06/2014` <dbl>, `19/06/2014` <dbl>, `03/07/2014` <dbl>,
## #   `18/07/2014` <dbl>, `01/08/2014` <dbl>, ...
```


5. Pivot the data back so that the dates are data and not column names.


```r
sydneybeaches_long_2 <- 
  sydneybeaches_wide %>% 
  pivot_longer(-site, 
               names_to = "date",
               values_to = "enterococci_cfu_100ml")
sydneybeaches_long_2
```

```
## # A tibble: 3,784 x 3
##    site           date       enterococci_cfu_100ml
##    <chr>          <chr>                      <dbl>
##  1 Clovelly Beach 02/01/2013                    19
##  2 Clovelly Beach 06/01/2013                     3
##  3 Clovelly Beach 12/01/2013                     2
##  4 Clovelly Beach 18/01/2013                    13
##  5 Clovelly Beach 30/01/2013                     8
##  6 Clovelly Beach 05/02/2013                     7
##  7 Clovelly Beach 11/02/2013                    11
##  8 Clovelly Beach 23/02/2013                    97
##  9 Clovelly Beach 07/03/2013                     3
## 10 Clovelly Beach 25/03/2013                     0
## # ... with 3,774 more rows
```


6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.


```r
sydneybeaches_long %>% 
  separate(date, into = c("day", "month", "year"), sep = "/")
```

```
## # A tibble: 3,690 x 5
##    site           day   month year  enterococci_cfu_100ml
##    <chr>          <chr> <chr> <chr>                 <dbl>
##  1 Clovelly Beach 02    01    2013                     19
##  2 Clovelly Beach 06    01    2013                      3
##  3 Clovelly Beach 12    01    2013                      2
##  4 Clovelly Beach 18    01    2013                     13
##  5 Clovelly Beach 30    01    2013                      8
##  6 Clovelly Beach 05    02    2013                      7
##  7 Clovelly Beach 11    02    2013                     11
##  8 Clovelly Beach 23    02    2013                     97
##  9 Clovelly Beach 07    03    2013                      3
## 10 Clovelly Beach 25    03    2013                      0
## # ... with 3,680 more rows
```

7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.


```r
mean_enterococci_long <-
  sydneybeaches_long %>% 
  separate(date, into = c("day", "month", "year"), sep = "/") %>% 
  group_by(site, year) %>% 
  summarize(mean_enterococci_cfu_100mL = mean(enterococci_cfu_100ml, na.rm = T))
```

```
## `summarise()` has grouped output by 'site'. You can override using the `.groups` argument.
```

```r
mean_enterococci_long
```

```
## # A tibble: 66 x 3
## # Groups:   site [11]
##    site         year  mean_enterococci_cfu_100mL
##    <chr>        <chr>                      <dbl>
##  1 Bondi Beach  2013                        32.2
##  2 Bondi Beach  2014                        11.1
##  3 Bondi Beach  2015                        14.3
##  4 Bondi Beach  2016                        19.4
##  5 Bondi Beach  2017                        13.2
##  6 Bondi Beach  2018                        22.9
##  7 Bronte Beach 2013                        26.8
##  8 Bronte Beach 2014                        17.5
##  9 Bronte Beach 2015                        23.6
## 10 Bronte Beach 2016                        61.3
## # ... with 56 more rows
```


8. Make the output from question 7 easier to read by pivoting it to wide format.

```r
mean_enterococci_wide <- 
  mean_enterococci_long %>% 
  pivot_wider(names_from = "year",
              values_from = "mean_enterococci_cfu_100mL")
mean_enterococci_wide
```

```
## # A tibble: 11 x 7
## # Groups:   site [11]
##    site                    `2013` `2014` `2015` `2016` `2017` `2018`
##    <chr>                    <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
##  1 Bondi Beach              32.2   11.1   14.3    19.4  13.2   22.9 
##  2 Bronte Beach             26.8   17.5   23.6    61.3  16.8   43.4 
##  3 Clovelly Beach            9.28  13.8    8.82   11.3   7.93  10.6 
##  4 Coogee Beach             39.7   52.6   40.3    59.5  20.7   21.6 
##  5 Gordons Bay (East)       24.8   16.7   36.2    39.0  13.7   17.6 
##  6 Little Bay Beach        122.    19.5   25.5    31.2  18.2   59.1 
##  7 Malabar Beach           101.    54.5   66.9    91.0  49.8   38.0 
##  8 Maroubra Beach           47.1    9.23  14.5    26.6  11.6    9.21
##  9 South Maroubra Beach     39.3   14.9    8.25   10.7   8.26  12.5 
## 10 South Maroubra Rockpool  96.4   40.6   47.3    59.3  46.9  112.  
## 11 Tamarama Beach           29.7   39.6   57.0    50.3  20.4   15.5
```



9. What was the most polluted beach in 2018?


```r
mean_enterococci_long %>% 
  filter(year == "2018") %>% 
  arrange(desc(mean_enterococci_cfu_100mL))
```

```
## # A tibble: 11 x 3
## # Groups:   site [11]
##    site                    year  mean_enterococci_cfu_100mL
##    <chr>                   <chr>                      <dbl>
##  1 South Maroubra Rockpool 2018                      112.  
##  2 Little Bay Beach        2018                       59.1 
##  3 Bronte Beach            2018                       43.4 
##  4 Malabar Beach           2018                       38.0 
##  5 Bondi Beach             2018                       22.9 
##  6 Coogee Beach            2018                       21.6 
##  7 Gordons Bay (East)      2018                       17.6 
##  8 Tamarama Beach          2018                       15.5 
##  9 South Maroubra Beach    2018                       12.5 
## 10 Clovelly Beach          2018                       10.6 
## 11 Maroubra Beach          2018                        9.21
```

10. Perform one analysis of your choice on the data.

```r
mean_enterococci_wide %>% 
  select(site, "2016", "2017", "2018") %>% 
  arrange(desc(.))
```

```
## # A tibble: 11 x 4
## # Groups:   site [11]
##    site                    `2016` `2017` `2018`
##    <chr>                    <dbl>  <dbl>  <dbl>
##  1 Tamarama Beach            50.3  20.4   15.5 
##  2 South Maroubra Rockpool   59.3  46.9  112.  
##  3 South Maroubra Beach      10.7   8.26  12.5 
##  4 Maroubra Beach            26.6  11.6    9.21
##  5 Malabar Beach             91.0  49.8   38.0 
##  6 Little Bay Beach          31.2  18.2   59.1 
##  7 Gordons Bay (East)        39.0  13.7   17.6 
##  8 Coogee Beach              59.5  20.7   21.6 
##  9 Clovelly Beach            11.3   7.93  10.6 
## 10 Bronte Beach              61.3  16.8   43.4 
## 11 Bondi Beach               19.4  13.2   22.9
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
