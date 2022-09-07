---
title: "Task4"
author: "Tyler"
date: "9/6/2022"
output: 
  html_document:
    keep_md: yes
---



```r
library(tidyverse)
```

```
## Warning: package 'tidyverse' was built under R version 4.1.3
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.2 --
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.1     v forcats 0.5.1
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
iris_data <- as_tibble (iris)
iris_data
```

```
## # A tibble: 150 x 5
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
##           <dbl>       <dbl>        <dbl>       <dbl> <fct>  
##  1          5.1         3.5          1.4         0.2 setosa 
##  2          4.9         3            1.4         0.2 setosa 
##  3          4.7         3.2          1.3         0.2 setosa 
##  4          4.6         3.1          1.5         0.2 setosa 
##  5          5           3.6          1.4         0.2 setosa 
##  6          5.4         3.9          1.7         0.4 setosa 
##  7          4.6         3.4          1.4         0.3 setosa 
##  8          5           3.4          1.5         0.2 setosa 
##  9          4.4         2.9          1.4         0.2 setosa 
## 10          4.9         3.1          1.5         0.1 setosa 
## # ... with 140 more rows
```


```r
a.sepal<-arrange(iris, Sepal.Length)
head(a.sepal,10)
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1           4.3         3.0          1.1         0.1  setosa
## 2           4.4         2.9          1.4         0.2  setosa
## 3           4.4         3.0          1.3         0.2  setosa
## 4           4.4         3.2          1.3         0.2  setosa
## 5           4.5         2.3          1.3         0.3  setosa
## 6           4.6         3.1          1.5         0.2  setosa
## 7           4.6         3.4          1.4         0.3  setosa
## 8           4.6         3.6          1.0         0.2  setosa
## 9           4.6         3.2          1.4         0.2  setosa
## 10          4.7         3.2          1.3         0.2  setosa
```



```r
testdat<-select(iris_data,Species,Petal.Width)
testdat
```

```
## # A tibble: 150 x 2
##    Species Petal.Width
##    <fct>         <dbl>
##  1 setosa          0.2
##  2 setosa          0.2
##  3 setosa          0.2
##  4 setosa          0.2
##  5 setosa          0.2
##  6 setosa          0.4
##  7 setosa          0.3
##  8 setosa          0.2
##  9 setosa          0.2
## 10 setosa          0.1
## # ... with 140 more rows
```


```r
Species_mean <- iris_data %>%
  group_by(Species)%>%
  summarise(mean(Petal.Width),mean(Petal.Length),mean(Sepal.Length), mean(Sepal.Width), na.rm =TRUE)
Species_mean
```

```
## # A tibble: 3 x 6
##   Species    `mean(Petal.Wi~` `mean(Petal.Le~` `mean(Sepal.Le~` `mean(Sepal.Wi~`
##   <fct>                 <dbl>            <dbl>            <dbl>            <dbl>
## 1 setosa                0.246             1.46             5.01             3.43
## 2 versicolor            1.33              4.26             5.94             2.77
## 3 virginica             2.03              5.55             6.59             2.97
## # ... with 1 more variable: na.rm <lgl>
```



```r
iris_size <- iris_data %>%
  mutate(Sepal.Size = 
           case_when(
             Sepal.Length < 5 ~ "small",
             Sepal.Length >= 5 & Sepal.Length < 6.5 ~ "medium",
             Sepal.Length >= 6.5 ~ "large"))
head(iris_size)   
```

```
## # A tibble: 6 x 6
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species Sepal.Size
##          <dbl>       <dbl>        <dbl>       <dbl> <fct>   <chr>     
## 1          5.1         3.5          1.4         0.2 setosa  medium    
## 2          4.9         3            1.4         0.2 setosa  small     
## 3          4.7         3.2          1.3         0.2 setosa  small     
## 4          4.6         3.1          1.5         0.2 setosa  small     
## 5          5           3.6          1.4         0.2 setosa  medium    
## 6          5.4         3.9          1.7         0.4 setosa  medium
```


```r
iris_ranking<-arrange(iris_data, desc(Petal.Length))
mutate(iris_ranking, rank=min_rank(Petal.Length))
```

```
## # A tibble: 150 x 6
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species    rank
##           <dbl>       <dbl>        <dbl>       <dbl> <fct>     <int>
##  1          7.7         2.6          6.9         2.3 virginica   150
##  2          7.7         3.8          6.7         2.2 virginica   148
##  3          7.7         2.8          6.7         2   virginica   148
##  4          7.6         3            6.6         2.1 virginica   147
##  5          7.9         3.8          6.4         2   virginica   146
##  6          7.3         2.9          6.3         1.8 virginica   145
##  7          7.2         3.6          6.1         2.5 virginica   142
##  8          7.4         2.8          6.1         1.9 virginica   142
##  9          7.7         3            6.1         2.3 virginica   142
## 10          6.3         3.3          6           2.5 virginica   140
## # ... with 140 more rows
```


```r
?summarise_all
```

```
## starting httpd help server ... done
```

```r
means_sd_species <- iris_data %>%
  group_by(Species) %>%
  summarize_all(list(Mean=mean, Std_dev = sd))
means_sd_species
```

```
## # A tibble: 3 x 9
##   Species    Sepal.Length_Me~ Sepal.Width_Mean Petal.Length_Me~ Petal.Width_Mean
##   <fct>                 <dbl>            <dbl>            <dbl>            <dbl>
## 1 setosa                 5.01             3.43             1.46            0.246
## 2 versicolor             5.94             2.77             4.26            1.33 
## 3 virginica              6.59             2.97             5.55            2.03 
## # ... with 4 more variables: Sepal.Length_Std_dev <dbl>,
## #   Sepal.Width_Std_dev <dbl>, Petal.Length_Std_dev <dbl>,
## #   Petal.Width_Std_dev <dbl>
```
###Why is male testostrone continuing to decline over the years?
###Has social media had an impact on the mental health of the youth?
###Is the increase of plastics in our food caused an increase in health issues?
###Feedback 5-10 people: When I asked people about my questions there were many intresting comments. The first question most people had theroies that it is lack of excerise and the decline in healthy eating has caused a decrease in testostrone. For the next question, every person said that socail media has had a major impact on mental health and there are plenty of stuides that can prove it. For my final question a few people brought up the recent article that microplastics have been found in human blood. Every person said that they are sure plastics have negative health effects. 
###Professtional Summary: I talked to a doctor who is a friend of my mom about these questions. He told me that all the questions I have asked is exactly what the medical field is trying to answer today. The doctor told me that microplatics are one of the major contributing factors to testostrone decline. He said that plastic shrink testicals and disrupt hormones in both males and females. It is becoming a major health issue that cannot be avovided because plastics are used in everything. He also said that anti-depressent use is at an all time high, and siad that social media has a big impact on the youth. He told me that he has young men and women come to him everyday asking for a perscription to fix thier depresstion. 
