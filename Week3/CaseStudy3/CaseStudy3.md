---
title: "CaseStudy3"
author: "Tyler"
date: "9/12/2022"
output: 
 html_document:
   keep_md: TRUE
---




```r
library(gapminder)
```

```
## Warning: package 'gapminder' was built under R version 4.1.3
```

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(ggplot2)
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 4.1.3
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.2 --
```

```
## v tibble  3.1.6     v purrr   0.3.4
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.1     v forcats 0.5.1
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
aveweath <-  filter(gapminder, country != "Kuwait")
view(aveweath)
```



```r
ggplot(data = aveweath) +
  geom_point(mapping = aes(x = lifeExp, y = gdpPercap, color = continent, size = pop)) +
  facet_wrap(~ year, nrow = 1) +
  scale_y_continuous(trans = "sqrt") +
  xlab("Life Expectancy") + ylab("GDP Per Capita") +
  theme_bw()
```

![](CaseStudy3_files/figure-html/unnamed-chunk-3-1.png)<!-- -->



```r
Wieght <- aveweath %>%
  group_by(year, continent) %>%
  summarise(average = weighted.mean(gdpPercap), population = pop/10000)
```

```
## `summarise()` has grouped output by 'year', 'continent'. You can override using
## the `.groups` argument.
```

```r
Wieght
```

```
## # A tibble: 1,692 x 4
## # Groups:   year, continent [60]
##     year continent average population
##    <int> <fct>       <dbl>      <dbl>
##  1  1952 Africa      1253.      928. 
##  2  1952 Africa      1253.      423. 
##  3  1952 Africa      1253.      174. 
##  4  1952 Africa      1253.       44.2
##  5  1952 Africa      1253.      447. 
##  6  1952 Africa      1253.      245. 
##  7  1952 Africa      1253.      501. 
##  8  1952 Africa      1253.      129. 
##  9  1952 Africa      1253.      268. 
## 10  1952 Africa      1253.       15.4
## # ... with 1,682 more rows
```



```r
ggplot(data = aveweath, mapping = aes(x = year, y = gdpPercap)) +
  geom_point(data = aveweath, mapping = aes(color = continent)) +
  geom_line(data = aveweath, mapping = aes(color = continent, group = country)) +
  geom_point(data = Wieght, aes(x = year, y = average, size = population)) +
  geom_line(data = Wieght, aes(x = year, y = average)) +
  facet_wrap(~ continent, nrow = 1) +
  xlab("Year") + ylab("GDP Per Capita") +
  scale_size_continuous(name = "Population (100k)", breaks = c(10000, 20000, 30000)) +
  theme_bw()
```

![](CaseStudy3_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

