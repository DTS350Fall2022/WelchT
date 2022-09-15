---
title: "Task6"
author: "Tyler"
date: "9/14/2022"
output:
 html_document:
   keep_md: TRUE
---

```r
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 4.1.3
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.2 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.1     v forcats 0.5.1
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x lubridate::as.difftime() masks base::as.difftime()
## x lubridate::date()        masks base::date()
## x dplyr::filter()          masks stats::filter()
## x lubridate::intersect()   masks base::intersect()
## x dplyr::lag()             masks stats::lag()
## x lubridate::setdiff()     masks base::setdiff()
## x lubridate::union()       masks base::union()
```

```r
library(ggrepel)
```

```
## Warning: package 'ggrepel' was built under R version 4.1.3
```



```r
largpwid <- iris %>%
  group_by(Species) %>%
  filter(row_number(desc(Petal.Width)) == 1)
```



```r
largpleng <- iris %>%
  group_by(Species) %>%
  filter(row_number(desc(Petal.Length)) == 1)
```



```r
ggplot(data = iris, mapping = aes(x = Sepal.Length, 
                                  y = Sepal.Width, 
                                  color = Species,
                                  shape = Species)) +
  geom_point() +
  geom_text(aes(color = Species, label = "Largest Petal Width",vjust = "center", hjust = "left"), data = largpwid, nudge_y=0.015) +
  geom_point(size = 3, shape = 1, color = "black", data = largpwid) +
  geom_text(aes(color = Species, label = "Largest Petal Length",vjust = "center", hjust = "left"), data = largpleng, nudge_y=-0.015) +
  geom_point(size = 3, shape = 1, color = "black", data = largpleng) +
  theme(legend.position = "bottom") +
  labs(x = "Sepal Length (cm)",
       y = "Sepal Width (cm)",
       title = "Different Iris Species Have Different Sepal Sizes",
       subtitle = "The Largest Petal Sizes For Each Species Do Not Correspond To The Largest Sepal Sizes")
```

![](Task-6_files/figure-html/unnamed-chunk-4-1.png)<!-- -->


```r
ScrabbleData <- tempfile()
download.file("https://media.githubusercontent.com/media/fivethirtyeight/data/master/scrabble-games/scrabble_games.csv",
ScrabbleData, mode = "wb")
Scrabble <- read_csv(ScrabbleData)
```

```
## Rows: 1542642 Columns: 19
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr   (2): winnername, losername
## dbl  (14): gameid, tourneyid, winnerid, winnerscore, winneroldrating, winner...
## lgl   (2): tie, lexicon
## date  (1): date
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
ScrabbleAll <- Scrabble %>%
  select(date, winnerscore, loserscore) %>%
  pivot_longer(c("winnerscore","loserscore"), names_to = "win-loss", values_to = "score") %>%
  filter(score > 0) %>%
  mutate(year = year(date),
         week = week(date)
  )
head(ScrabbleAll)
```

```
## # A tibble: 6 x 5
##   date       `win-loss`  score  year  week
##   <date>     <chr>       <dbl> <dbl> <dbl>
## 1 1999-01-15 winnerscore   521  1999     3
## 2 1999-01-15 loserscore    237  1999     3
## 3 1999-01-15 winnerscore   488  1999     3
## 4 1999-01-15 loserscore    262  1999     3
## 5 1999-01-15 winnerscore   465  1999     3
## 6 1999-01-15 loserscore    330  1999     3
```


```r
ScrabbleAverage <- ScrabbleAll %>%
group_by(year, week) %>%
summarise(avglength = mean (score), Date = min(date))
```

```
## `summarise()` has grouped output by 'year'. You can override using the `.groups`
## argument.
```

```r
head(ScrabbleAverage)
```

```
## # A tibble: 6 x 4
## # Groups:   year [2]
##    year  week avglength Date      
##   <dbl> <dbl>     <dbl> <date>    
## 1  1976    49      367. 1976-12-05
## 2  1977     5      324  1977-02-01
## 3  1977     9      317. 1977-02-27
## 4  1977    10      414. 1977-03-05
## 5  1977    13      371. 1977-03-26
## 6  1977    19      354. 1977-05-07
```


```r
after_average <- ScrabbleAll %>% 
group_by(year, week) %>%
filter(date > as.Date("2006-03-01")) %>%
summarise(avglength = mean (score), Date = min(date))
```

```
## `summarise()` has grouped output by 'year'. You can override using the `.groups`
## argument.
```


```r
before_average <- ScrabbleAll %>% 
group_by(year, week) %>%
filter(date <= as.Date("2006-03-01")) %>%
summarise(avglength = mean (score), Date = min(date))
```

```
## `summarise()` has grouped output by 'year'. You can override using the `.groups`
## argument.
```


```r
ScrabblePlot1 <- ggplot(data = ScrabbleAverage, mapping = aes(x = Date, y = avglength)) +
geom_point() +
geom_smooth(data = before_average, method = 'lm', color="orange") +
annotate("text", x=as.Date("2005-12-01"), y = 380, label = "Trend Before", color = "orange", size = 3) + geom_smooth(data = after_average, method = 'lm', color = "blue") +
annotate("text", x=as.Date("2006-07-15"), y = 380, label = "Trend After", color = "blue", size = 3) + 
geom_vline(xintercept=as.Date("2006-03-05"), linetype = "dotted") +
annotate ("text", x = as.Date("2006-03-05"), y = 390, label = "Dictionary\nUpdated", size = 3) +
labs(y = "Average Score", x = "", title = "Scrabble scores in the age of 'Q1' and 'ZA'", subtitle = 'weekly average scores before and after the addition of around 11,000 words to the Scrabble dictionary') +
coord_cartesian(ylim = c(355,405), expand = FALSE) +
scale_x_date(date_breaks = "3 month", 
limits = as.Date(c('1/6/2005', '1/9/2006'), format = "%d/%m/%Y"), 
date_labels = "%b-%y" ) + 
theme_bw()
ScrabblePlot1
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: Removed 542 rows containing non-finite values (stat_smooth).
```

```
## `geom_smooth()` using formula 'y ~ x'
```

```
## Warning: Removed 550 rows containing non-finite values (stat_smooth).
```

```
## Warning: Removed 1092 rows containing missing values (geom_point).
```

![](Task-6_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


