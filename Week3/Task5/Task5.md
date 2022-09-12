---
title: "Task 5"
author: "Tyler"
date: "9/11/2022"
output: 
  html_document:
      code_folding: 'hide'
      keep_md: true 
editor_options: 
  chunk_output_type: console
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
library(downloader)
```

```
## Warning: package 'downloader' was built under R version 4.1.3
```

```r
library(readxl)
```


```r
SoloData <- read_csv("solo-artist-followers.csv")
```

```
## Rows: 139 Columns: 5
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (5): name, band, followers, band_followers, follower_difference
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
BillboardData <- read_csv("billboard-hits.csv")
```

```
## Rows: 456 Columns: 5
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr  (3): name, band, title
## dbl  (1): peak_rank
## date (1): peak_date
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
view(SoloData)
view(BillboardData)
str(SoloData)
```

```
## spec_tbl_df [139 x 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ name               : chr [1:139] "Daron Jones" "Slim" "Q Parker" "JC Chasez" ...
##  $ band               : chr [1:139] "112" "112" "112" "*NSYNC" ...
##  $ followers          : chr [1:139] "1.28k" "2.14k" "3.51k" "30.8k" ...
##  $ band_followers     : chr [1:139] "783k" "783k" "783k" "1.44M" ...
##  $ follower_difference: chr [1:139] "-782k" "-781k" "-780k" "-1.41M" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   name = col_character(),
##   ..   band = col_character(),
##   ..   followers = col_character(),
##   ..   band_followers = col_character(),
##   ..   follower_difference = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

```r
str(BillboardData)
```

```
## spec_tbl_df [456 x 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ name     : chr [1:456] "*NSYNC" "*NSYNC" "*NSYNC" "*NSYNC" ...
##  $ band     : chr [1:456] NA NA NA NA ...
##  $ title    : chr [1:456] "It's Gonna Be Me" "Music Of My Heart" "Bye Bye Bye" "This I Promise You" ...
##  $ peak_date: Date[1:456], format: "2000-07-28" "1999-10-15" ...
##  $ peak_rank: num [1:456] 1 2 4 5 5 8 11 13 19 59 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   name = col_character(),
##   ..   band = col_character(),
##   ..   title = col_character(),
##   ..   peak_date = col_date(format = ""),
##   ..   peak_rank = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```
All the data types imported and were read in correctly 


```r
top_six <- BillboardData %>%
  group_by(name) %>%
  filter(n() > 6, band != "")
view(top_six)
```



```r
top_bands <- BillboardData %>%
  group_by(band) %>%
  filter(name %in% top_six$band)
head(top_bands)
```

```
## # A tibble: 6 x 5
## # Groups:   band [1]
##   name   band  title                     peak_date  peak_rank
##   <chr>  <chr> <chr>                     <date>         <dbl>
## 1 *NSYNC <NA>  It's Gonna Be Me          2000-07-28         1
## 2 *NSYNC <NA>  Music Of My Heart         1999-10-15         2
## 3 *NSYNC <NA>  Bye Bye Bye               2000-04-14         4
## 4 *NSYNC <NA>  This I Promise You        2000-12-01         5
## 5 *NSYNC <NA>  Girlfriend                2002-04-05         5
## 6 *NSYNC <NA>  A Little More Time On You 1999-02-26         8
```

```r
#Switch band names with the name data.
top_bands <-rename(top_bands, band = name, name = band)
head(top_bands)
```

```
## # A tibble: 6 x 5
## # Groups:   name [1]
##   band   name  title                     peak_date  peak_rank
##   <chr>  <chr> <chr>                     <date>         <dbl>
## 1 *NSYNC <NA>  It's Gonna Be Me          2000-07-28         1
## 2 *NSYNC <NA>  Music Of My Heart         1999-10-15         2
## 3 *NSYNC <NA>  Bye Bye Bye               2000-04-14         4
## 4 *NSYNC <NA>  This I Promise You        2000-12-01         5
## 5 *NSYNC <NA>  Girlfriend                2002-04-05         5
## 6 *NSYNC <NA>  A Little More Time On You 1999-02-26         8
```

```r
view(top_bands)
```


```r
ggplot(data = top_six, aes(x = peak_date, y = peak_rank, color = name, group = name)) +
  geom_point() +
  geom_line() +
  geom_point(data = top_bands, color = "black") +
  geom_line(data = top_bands, color = "black", linetype = "dotted") +
  facet_wrap(~band, scales = "free") +
  xlab("peak_rank") + ylab("peak_rank")
```

```
## geom_path: Each group consists of only one observation. Do you need to adjust
## the group aesthetic?
```

![](Task5_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
  theme_bw()
```

```
## List of 93
##  $ line                      :List of 6
##   ..$ colour       : chr "black"
##   ..$ size         : num 0.5
##   ..$ linetype     : num 1
##   ..$ lineend      : chr "butt"
##   ..$ arrow        : logi FALSE
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_line" "element"
##  $ rect                      :List of 5
##   ..$ fill         : chr "white"
##   ..$ colour       : chr "black"
##   ..$ size         : num 0.5
##   ..$ linetype     : num 1
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_rect" "element"
##  $ text                      :List of 11
##   ..$ family       : chr ""
##   ..$ face         : chr "plain"
##   ..$ colour       : chr "black"
##   ..$ size         : num 11
##   ..$ hjust        : num 0.5
##   ..$ vjust        : num 0.5
##   ..$ angle        : num 0
##   ..$ lineheight   : num 0.9
##   ..$ margin       : 'margin' num [1:4] 0points 0points 0points 0points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : logi FALSE
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ title                     : NULL
##  $ aspect.ratio              : NULL
##  $ axis.title                : NULL
##  $ axis.title.x              :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : NULL
##   ..$ vjust        : num 1
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 2.75points 0points 0points 0points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ axis.title.x.top          :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : NULL
##   ..$ vjust        : num 0
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 0points 0points 2.75points 0points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ axis.title.x.bottom       : NULL
##  $ axis.title.y              :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : NULL
##   ..$ vjust        : num 1
##   ..$ angle        : num 90
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 0points 2.75points 0points 0points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ axis.title.y.left         : NULL
##  $ axis.title.y.right        :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : NULL
##   ..$ vjust        : num 0
##   ..$ angle        : num -90
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 0points 0points 0points 2.75points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ axis.text                 :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : chr "grey30"
##   ..$ size         : 'rel' num 0.8
##   ..$ hjust        : NULL
##   ..$ vjust        : NULL
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : NULL
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ axis.text.x               :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : NULL
##   ..$ vjust        : num 1
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 2.2points 0points 0points 0points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ axis.text.x.top           :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : NULL
##   ..$ vjust        : num 0
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 0points 0points 2.2points 0points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ axis.text.x.bottom        : NULL
##  $ axis.text.y               :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : num 1
##   ..$ vjust        : NULL
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 0points 2.2points 0points 0points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ axis.text.y.left          : NULL
##  $ axis.text.y.right         :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : num 0
##   ..$ vjust        : NULL
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 0points 0points 0points 2.2points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ axis.ticks                :List of 6
##   ..$ colour       : chr "grey20"
##   ..$ size         : NULL
##   ..$ linetype     : NULL
##   ..$ lineend      : NULL
##   ..$ arrow        : logi FALSE
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_line" "element"
##  $ axis.ticks.x              : NULL
##  $ axis.ticks.x.top          : NULL
##  $ axis.ticks.x.bottom       : NULL
##  $ axis.ticks.y              : NULL
##  $ axis.ticks.y.left         : NULL
##  $ axis.ticks.y.right        : NULL
##  $ axis.ticks.length         : 'simpleUnit' num 2.75points
##   ..- attr(*, "unit")= int 8
##  $ axis.ticks.length.x       : NULL
##  $ axis.ticks.length.x.top   : NULL
##  $ axis.ticks.length.x.bottom: NULL
##  $ axis.ticks.length.y       : NULL
##  $ axis.ticks.length.y.left  : NULL
##  $ axis.ticks.length.y.right : NULL
##  $ axis.line                 : list()
##   ..- attr(*, "class")= chr [1:2] "element_blank" "element"
##  $ axis.line.x               : NULL
##  $ axis.line.x.top           : NULL
##  $ axis.line.x.bottom        : NULL
##  $ axis.line.y               : NULL
##  $ axis.line.y.left          : NULL
##  $ axis.line.y.right         : NULL
##  $ legend.background         :List of 5
##   ..$ fill         : NULL
##   ..$ colour       : logi NA
##   ..$ size         : NULL
##   ..$ linetype     : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_rect" "element"
##  $ legend.margin             : 'margin' num [1:4] 5.5points 5.5points 5.5points 5.5points
##   ..- attr(*, "unit")= int 8
##  $ legend.spacing            : 'simpleUnit' num 11points
##   ..- attr(*, "unit")= int 8
##  $ legend.spacing.x          : NULL
##  $ legend.spacing.y          : NULL
##  $ legend.key                :List of 5
##   ..$ fill         : chr "white"
##   ..$ colour       : logi NA
##   ..$ size         : NULL
##   ..$ linetype     : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_rect" "element"
##  $ legend.key.size           : 'simpleUnit' num 1.2lines
##   ..- attr(*, "unit")= int 3
##  $ legend.key.height         : NULL
##  $ legend.key.width          : NULL
##  $ legend.text               :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : 'rel' num 0.8
##   ..$ hjust        : NULL
##   ..$ vjust        : NULL
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : NULL
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ legend.text.align         : NULL
##  $ legend.title              :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : num 0
##   ..$ vjust        : NULL
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : NULL
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ legend.title.align        : NULL
##  $ legend.position           : chr "right"
##  $ legend.direction          : NULL
##  $ legend.justification      : chr "center"
##  $ legend.box                : NULL
##  $ legend.box.just           : NULL
##  $ legend.box.margin         : 'margin' num [1:4] 0cm 0cm 0cm 0cm
##   ..- attr(*, "unit")= int 1
##  $ legend.box.background     : list()
##   ..- attr(*, "class")= chr [1:2] "element_blank" "element"
##  $ legend.box.spacing        : 'simpleUnit' num 11points
##   ..- attr(*, "unit")= int 8
##  $ panel.background          :List of 5
##   ..$ fill         : chr "white"
##   ..$ colour       : logi NA
##   ..$ size         : NULL
##   ..$ linetype     : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_rect" "element"
##  $ panel.border              :List of 5
##   ..$ fill         : logi NA
##   ..$ colour       : chr "grey20"
##   ..$ size         : NULL
##   ..$ linetype     : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_rect" "element"
##  $ panel.spacing             : 'simpleUnit' num 5.5points
##   ..- attr(*, "unit")= int 8
##  $ panel.spacing.x           : NULL
##  $ panel.spacing.y           : NULL
##  $ panel.grid                :List of 6
##   ..$ colour       : chr "grey92"
##   ..$ size         : NULL
##   ..$ linetype     : NULL
##   ..$ lineend      : NULL
##   ..$ arrow        : logi FALSE
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_line" "element"
##  $ panel.grid.major          : NULL
##  $ panel.grid.minor          :List of 6
##   ..$ colour       : NULL
##   ..$ size         : 'rel' num 0.5
##   ..$ linetype     : NULL
##   ..$ lineend      : NULL
##   ..$ arrow        : logi FALSE
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_line" "element"
##  $ panel.grid.major.x        : NULL
##  $ panel.grid.major.y        : NULL
##  $ panel.grid.minor.x        : NULL
##  $ panel.grid.minor.y        : NULL
##  $ panel.ontop               : logi FALSE
##  $ plot.background           :List of 5
##   ..$ fill         : NULL
##   ..$ colour       : chr "white"
##   ..$ size         : NULL
##   ..$ linetype     : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_rect" "element"
##  $ plot.title                :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : 'rel' num 1.2
##   ..$ hjust        : num 0
##   ..$ vjust        : num 1
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 0points 0points 5.5points 0points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ plot.title.position       : chr "panel"
##  $ plot.subtitle             :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : num 0
##   ..$ vjust        : num 1
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 0points 0points 5.5points 0points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ plot.caption              :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : 'rel' num 0.8
##   ..$ hjust        : num 1
##   ..$ vjust        : num 1
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 5.5points 0points 0points 0points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ plot.caption.position     : chr "panel"
##  $ plot.tag                  :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : 'rel' num 1.2
##   ..$ hjust        : num 0.5
##   ..$ vjust        : num 0.5
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : NULL
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ plot.tag.position         : chr "topleft"
##  $ plot.margin               : 'margin' num [1:4] 5.5points 5.5points 5.5points 5.5points
##   ..- attr(*, "unit")= int 8
##  $ strip.background          :List of 5
##   ..$ fill         : chr "grey85"
##   ..$ colour       : chr "grey20"
##   ..$ size         : NULL
##   ..$ linetype     : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_rect" "element"
##  $ strip.background.x        : NULL
##  $ strip.background.y        : NULL
##  $ strip.placement           : chr "inside"
##  $ strip.text                :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : chr "grey10"
##   ..$ size         : 'rel' num 0.8
##   ..$ hjust        : NULL
##   ..$ vjust        : NULL
##   ..$ angle        : NULL
##   ..$ lineheight   : NULL
##   ..$ margin       : 'margin' num [1:4] 4.4points 4.4points 4.4points 4.4points
##   .. ..- attr(*, "unit")= int 8
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ strip.text.x              : NULL
##  $ strip.text.y              :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : NULL
##   ..$ vjust        : NULL
##   ..$ angle        : num -90
##   ..$ lineheight   : NULL
##   ..$ margin       : NULL
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  $ strip.switch.pad.grid     : 'simpleUnit' num 2.75points
##   ..- attr(*, "unit")= int 8
##  $ strip.switch.pad.wrap     : 'simpleUnit' num 2.75points
##   ..- attr(*, "unit")= int 8
##  $ strip.text.y.left         :List of 11
##   ..$ family       : NULL
##   ..$ face         : NULL
##   ..$ colour       : NULL
##   ..$ size         : NULL
##   ..$ hjust        : NULL
##   ..$ vjust        : NULL
##   ..$ angle        : num 90
##   ..$ lineheight   : NULL
##   ..$ margin       : NULL
##   ..$ debug        : NULL
##   ..$ inherit.blank: logi TRUE
##   ..- attr(*, "class")= chr [1:2] "element_text" "element"
##  - attr(*, "class")= chr [1:2] "theme" "gg"
##  - attr(*, "complete")= logi TRUE
##  - attr(*, "validate")= logi TRUE
```
In genral it seems that all the bands gained listens as the years went on. Destiny's child seem to have massive jumps
randomly. I assume that when they realsed new music or some sort of seasonal thing. The Jonas Brothers lost the most 
listens as the years went on epsically at the peak around 2011.One Direction seems to be the most consistant and is 
an overall great. 

```r
Rentals<-read_csv("House_Rent_Dataset.csv")
```

```
## Rows: 4746 Columns: 12
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr  (7): Floor, Area Type, Area Locality, City, Furnishing Status, Tenant P...
## dbl  (4): BHK, Rent, Size, Bathroom
## date (1): Posted On
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
head(Rentals)
```

```
## # A tibble: 6 x 12
##   `Posted On`   BHK  Rent  Size Floor          `Area Type` `Area Locality` City 
##   <date>      <dbl> <dbl> <dbl> <chr>          <chr>       <chr>           <chr>
## 1 2022-05-18      2 10000  1100 Ground out of~ Super Area  Bandel          Kolk~
## 2 2022-05-13      2 20000   800 1 out of 3     Super Area  Phool Bagan, K~ Kolk~
## 3 2022-05-16      2 17000  1000 1 out of 3     Super Area  Salt Lake City~ Kolk~
## 4 2022-07-04      2 10000   800 1 out of 2     Super Area  Dumdum Park     Kolk~
## 5 2022-05-09      2  7500   850 1 out of 2     Carpet Area South Dum Dum   Kolk~
## 6 2022-04-29      2  7000   600 Ground out of~ Super Area  Thakurpukur     Kolk~
## # ... with 4 more variables: `Furnishing Status` <chr>,
## #   `Tenant Preferred` <chr>, Bathroom <dbl>, `Point of Contact` <chr>
```
The data above includes 4,700 apartments, houses, and other building availble fron rent. The data comes from the country of India that has a booming housing industry. 
https://www.kaggle.com/datasets/iamsouravbanerjee/house-rent-prediction-dataset?resource=download 

```r
Tempuratures<-read_csv("Climate Data.csv")
```

```
## New names:
## * Land -> Land...4
## * Ocean -> Ocean...5
## * Land -> Land...7
## * Ocean -> Ocean...8
## * Land -> Land...10
## * ...
```

```
## Rows: 2105 Columns: 30
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr  (2): Year, Troposphere
## dbl (28): Mo, Globe, Land...4, Ocean...5, NH, Land...7, Ocean...8, SH, Land....
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
head(Tempuratures)
```

```
## # A tibble: 6 x 30
##   Year     Mo Globe Land...4 Ocean...5    NH Land...7 Ocean...8    SH Land...10
##   <chr> <dbl> <dbl>    <dbl>     <dbl> <dbl>    <dbl>     <dbl> <dbl>     <dbl>
## 1 1978     12 -0.48    -0.51     -0.47 -0.44    -0.46     -0.42 -0.52     -0.62
## 2 1979      1 -0.47    -0.64     -0.41 -0.64    -0.86     -0.5  -0.31     -0.13
## 3 1979      2 -0.43    -0.56     -0.39 -0.47    -0.57     -0.41 -0.39     -0.53
## 4 1979      3 -0.38    -0.51     -0.33 -0.46    -0.51     -0.44 -0.3      -0.53
## 5 1979      4 -0.4     -0.57     -0.34 -0.47    -0.62     -0.37 -0.34     -0.46
## 6 1979      5 -0.4     -0.56     -0.33 -0.52    -0.54     -0.52 -0.27     -0.62
## # ... with 20 more variables: Ocean...11 <dbl>, Trpcs <dbl>, Land...13 <dbl>,
## #   Ocean...14 <dbl>, NoExt <dbl>, Land...16 <dbl>, Ocean...17 <dbl>,
## #   SoExt <dbl>, Land...19 <dbl>, Ocean...20 <dbl>, NoPol <dbl>,
## #   Land...22 <dbl>, Ocean...23 <dbl>, SoPol <dbl>, Land...25 <dbl>,
## #   Ocean...26 <dbl>, USA48 <dbl>, USA49 <dbl>, AUST <dbl>, Troposphere <chr>
```
Every month, John Christy and Dr. Roy Spencer update global temperature datasets that represent the piecing together of the temperature data from a total of fifteen instruments flying on different satellites over the years.
https://www.kaggle.com/datasets/csafrit2/latest-global-temperatures 

```r
Crypto<-read_csv("Crypto Prices - Sheet1.csv")
```

```
## Rows: 500 Columns: 9
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (8): Name, Price, 1h%, 24h%, 7d%, Market Cap, Volume(24h), Circulating S...
## dbl (1): #
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
head(Crypto)
```

```
## # A tibble: 6 x 9
##     `#` Name                Price  `1h%` `24h%` `7d%` `Market Cap` `Volume(24h)`
##   <dbl> <chr>               <chr>  <chr> <chr>  <chr> <chr>        <chr>        
## 1     1 "Bitcoin\nBTC"      $22,2~ 0.36% 2.82%  12.9~ $425,551,34~ "$45,901,123~
## 2     2 "Ethereum\nETH"     $1,75~ 0.38% 1.03%  12.0~ $214,273,15~ "$15,629,033~
## 3     3 "Tether\nUSDT"      $1.00  0.00% 0.00%  0.03% $67,738,709~ "$60,727,804~
## 4     4 "USD Coin\nUSDC"    $1.00  0.00% 0.02%  0.01% $51,581,244~ "$6,560,114,~
## 5     5 "BNB\nBNB"          $297.~ 0.06% 0.53%  8.45% $48,000,545~ "$848,231,93~
## 6     6 "Binance USD\nBUSD" $1.00  0.04% 0.01%  0.01% $20,061,675~ "$10,087,128~
## # ... with 1 more variable: `Circulating Supply` <chr>
```
his is the complete dataset of crypto prices which will updated daily. The file contains prices, volume, market capacity and circulating supply of all currencies. This file covers 500 currencies prices in USD.
https://www.kaggle.com/datasets/batrosjamali/cryptocurrency-prices-complete-data
