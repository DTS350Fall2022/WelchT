---
title: "Task&7"
author: "Tyler"
date: "9/19/2022"
output: 
 html_document:
   keep_md: TRUE
---


```r
library(ggplot2)
```

```
## Warning in register(): Can't find generic `scale_type` in package ggplot2 to
## register S3 method.
```

```r
library(readr)
library(knitr)
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 4.1.3
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.2 --
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v purrr   0.3.4     v forcats 0.5.1
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(dplyr)
library(forcats)
library(downloader)
```

```
## Warning: package 'downloader' was built under R version 4.1.3
```




```r
download.file("https://raw.githubusercontent.com/hadley/r4ds/master/data/heights.csv", "heights")

Height <- read_csv("heights")
```

```
## Rows: 1192 Columns: 6
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (2): sex, race
## dbl (4): earn, height, ed, age
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
Height
```

```
## # A tibble: 1,192 x 6
##     earn height sex       ed   age race    
##    <dbl>  <dbl> <chr>  <dbl> <dbl> <chr>   
##  1 50000   74.4 male      16    45 white   
##  2 60000   65.5 female    16    58 white   
##  3 30000   63.6 female    16    29 white   
##  4 50000   63.1 female    16    91 other   
##  5 51000   63.4 female    17    39 white   
##  6  9000   64.4 female    15    26 white   
##  7 29000   61.7 female    12    49 white   
##  8 32000   72.7 male      17    46 white   
##  9  2000   72.0 male      15    21 hispanic
## 10 27000   72.2 male      12    26 white   
## # ... with 1,182 more rows
```



```r
HeightEd <- Height %>%
  mutate(ed = as.factor(ed)) %>%
  mutate(ed = fct_recode(ed,
                         "primary" = "3",
                         "primary" = "4",
                         "primary" = "5",
                         "primary" = "6",
                         "secondary" = "7",
                         "secondary" = "8",
                         "secondary" = "9",
                         "secondary" = "10",
                         "secondary" = "11",
                         "highschoolgrad" = "12",
                         "associates" = "13",
                         "associates" = "14",
                         "associates" = "15",
                         "bachelors" =  "16",
                         "mastersdoctorate" = "17",
                         "mastersdoctorate" = "18"))
```


```r
HeightEd1 <- HeightEd %>%
  group_by(race) %>%
  count(ed)

HeightEd1
```

```
## # A tibble: 24 x 3
## # Groups:   race [4]
##    race     ed                   n
##    <chr>    <fct>            <int>
##  1 black    primary              2
##  2 black    secondary           14
##  3 black    highschoolgrad      36
##  4 black    associates          38
##  5 black    bachelors           13
##  6 black    mastersdoctorate     9
##  7 hispanic primary              3
##  8 hispanic secondary            6
##  9 hispanic highschoolgrad      19
## 10 hispanic associates          22
## # ... with 14 more rows
```


```r
ggplot(HeightEd1, mapping=aes(ed,n, fill=race ))+
  geom_bar(stat="identity", position = "dodge")
```

![](Task7_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


```r
HeightEd2 <- HeightEd %>%
  group_by(sex) %>%
  count(ed)

HeightEd2
```

```
## # A tibble: 12 x 3
## # Groups:   sex [2]
##    sex    ed                   n
##    <chr>  <fct>            <int>
##  1 female primary              8
##  2 female secondary           48
##  3 female highschoolgrad     255
##  4 female associates         212
##  5 female bachelors           91
##  6 female mastersdoctorate    73
##  7 male   primary              4
##  8 male   secondary           47
##  9 male   highschoolgrad     177
## 10 male   associates         131
## 11 male   bachelors           78
## 12 male   mastersdoctorate    68
```


```r
ggplot(HeightEd2, mapping=aes(ed,n, fill=sex ))+
  geom_bar(stat="identity", position = "dodge")
```

![](Task7_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


```r
HeightEd3 <- HeightEd %>%
  group_by(race) %>%
  count(earn)

HeightEd3
```

```
## # A tibble: 228 x 3
## # Groups:   race [4]
##    race   earn     n
##    <chr> <dbl> <int>
##  1 black   600     1
##  2 black  1000     2
##  3 black  1200     1
##  4 black  2100     1
##  5 black  3000     3
##  6 black  3192     1
##  7 black  4000     2
##  8 black  4700     1
##  9 black  5000     2
## 10 black  6000     1
## # ... with 218 more rows
```


```r
ggplot(data=HeightEd3, mapping=aes(x=earn,y=n, color=factor(race)))+
  geom_point()+
  labs(y="Number of people", x="Earnings")
```

![](Task7_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

In the data set above on the x axis it shows the amount earned by the indivual and on the y it shows the amount of indiviuals that earned it. THe different color points represent the race of the person or people at that point. What I learned is that white indivuals have a lot more top earners than any other race. Also even at low incomes white people have a lot more indviuals at those points than any other race. 

