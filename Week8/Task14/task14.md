---
title: "Task14"
author: "Tyler"
date: "10/19/2022"
output:  
  html_document:
   keep_md: TRUE
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
library(knitr)
library(downloader)
```

```
## Warning: package 'downloader' was built under R version 4.1.3
```

```r
library(dplyr)
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
## 
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(ggplot2)
library(grid)
library(readr) 
library(haven)
library(readxl)
library(stringi)
library(stringr)
```




```r
bob <- tempfile()
download("https://raw.githubusercontent.com/WJC-Data-Science/DTS350/master/bible.csv", bob, mode = "wb")
bib1 <- read_csv(bob)
```

```
## Rows: 31102 Columns: 17
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr (9): volume_title, book_title, volume_long_title, book_long_title, volum...
## dbl (6): volume_id, book_id, chapter_id, verse_id, chapter_number, verse_number
## lgl (2): volume_subtitle, book_subtitle
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
head(bib1)
```

```
## # A tibble: 6 x 17
##   volume_id book_id chapter_id verse_id volume_title book_title volume_long_tit~
##       <dbl>   <dbl>      <dbl>    <dbl> <chr>        <chr>      <chr>           
## 1         1       1          1        1 Old Testame~ Genesis    The Old Testame~
## 2         1       1          1        2 Old Testame~ Genesis    The Old Testame~
## 3         1       1          1        3 Old Testame~ Genesis    The Old Testame~
## 4         1       1          1        4 Old Testame~ Genesis    The Old Testame~
## 5         1       1          1        5 Old Testame~ Genesis    The Old Testame~
## 6         1       1          1        6 Old Testame~ Genesis    The Old Testame~
## # ... with 10 more variables: book_long_title <chr>, volume_subtitle <lgl>,
## #   book_subtitle <lgl>, volume_short_title <chr>, book_short_title <chr>,
## #   chapter_number <dbl>, verse_number <dbl>, scripture_text <chr>,
## #   verse_title <chr>, verse_short_title <chr>
```

```r
str(bib1)
```

```
## spec_tbl_df [31,102 x 17] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ volume_id         : num [1:31102] 1 1 1 1 1 1 1 1 1 1 ...
##  $ book_id           : num [1:31102] 1 1 1 1 1 1 1 1 1 1 ...
##  $ chapter_id        : num [1:31102] 1 1 1 1 1 1 1 1 1 1 ...
##  $ verse_id          : num [1:31102] 1 2 3 4 5 6 7 8 9 10 ...
##  $ volume_title      : chr [1:31102] "Old Testament" "Old Testament" "Old Testament" "Old Testament" ...
##  $ book_title        : chr [1:31102] "Genesis" "Genesis" "Genesis" "Genesis" ...
##  $ volume_long_title : chr [1:31102] "The Old Testament" "The Old Testament" "The Old Testament" "The Old Testament" ...
##  $ book_long_title   : chr [1:31102] "The First Book of Moses called Genesis" "The First Book of Moses called Genesis" "The First Book of Moses called Genesis" "The First Book of Moses called Genesis" ...
##  $ volume_subtitle   : logi [1:31102] NA NA NA NA NA NA ...
##  $ book_subtitle     : logi [1:31102] NA NA NA NA NA NA ...
##  $ volume_short_title: chr [1:31102] "OT" "OT" "OT" "OT" ...
##  $ book_short_title  : chr [1:31102] "Gen." "Gen." "Gen." "Gen." ...
##  $ chapter_number    : num [1:31102] 1 1 1 1 1 1 1 1 1 1 ...
##  $ verse_number      : num [1:31102] 1 2 3 4 5 6 7 8 9 10 ...
##  $ scripture_text    : chr [1:31102] "IN the beginning God created the heaven and the earth." "And the earth was without form, and void; and darkness was upon the face of the deep. And the Spirit of God mov"| __truncated__ "And God said, Let there be light: and there was light." "And God saw the light, that it was good: and God divided the light from the darkness." ...
##  $ verse_title       : chr [1:31102] "Genesis 1:1" "Genesis 1:2" "Genesis 1:3" "Genesis 1:4" ...
##  $ verse_short_title : chr [1:31102] "Gen. 1:1" "Gen. 1:2" "Gen. 1:3" "Gen. 1:4" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   volume_id = col_double(),
##   ..   book_id = col_double(),
##   ..   chapter_id = col_double(),
##   ..   verse_id = col_double(),
##   ..   volume_title = col_character(),
##   ..   book_title = col_character(),
##   ..   volume_long_title = col_character(),
##   ..   book_long_title = col_character(),
##   ..   volume_subtitle = col_logical(),
##   ..   book_subtitle = col_logical(),
##   ..   volume_short_title = col_character(),
##   ..   book_short_title = col_character(),
##   ..   chapter_number = col_double(),
##   ..   verse_number = col_double(),
##   ..   scripture_text = col_character(),
##   ..   verse_title = col_character(),
##   ..   verse_short_title = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```


```r
old <- bib1 %>%
  filter(volume_long_title == "The Old Testament") %>%
  select(scripture_text)
head(old)
```

```
## # A tibble: 6 x 1
##   scripture_text                                                                
##   <chr>                                                                         
## 1 IN the beginning God created the heaven and the earth.                        
## 2 And the earth was without form, and void; and darkness was upon the face of t~
## 3 And God said, Let there be light: and there was light.                        
## 4 And God saw the light, that it was good: and God divided the light from the d~
## 5 And God called the light Day, and the darkness he called Night. And the eveni~
## 6 And God said, Let there be a firmament in the midst of the waters, and let it~
```

```r
new <- bib1 %>%
  filter(volume_long_title == "The New Testament") %>%
  select(scripture_text)
head(new)
```

```
## # A tibble: 6 x 1
##   scripture_text                                                                
##   <chr>                                                                         
## 1 THE book of the generation of Jesus Christ, the son of David, the son of Abra~
## 2 Abraham begat Isaac; and Isaac begat Jacob; and Jacob begat Judas and his bre~
## 3 And Judas begat Phares and Zara of Thamar; and Phares begat Esrom; and Esrom ~
## 4 And Aram begat Aminadab; and Aminadab begat Naasson; and Naasson begat Salmon;
## 5 And Salmon begat Booz of Rachab; and Booz begat Obed of Ruth; and Obed begat ~
## 6 And Jesse begat David the king; and David the king begat Solomon of her that ~
```


```r
versel <- function(df) {
  vlength <- vector("integer", 0)
  for(row in df) {
    vlength <- append(vlength, str_length(row))
  }
  vlength
}
```


```r
mean(versel(old))
```

```
## [1] 136.7845
```

```r
mean(versel(new))
```

```
## [1] 118.3265
```


```r
str_length(str_extract_all(old, "(?i)lord"))
```

```
## Warning in stri_extract_all_regex(string, pattern, simplify = simplify, :
## argument is not an atomic vector; coercing
```

```
## Warning in stri_length(string): argument is not an atomic vector; coercing
```

```
## [1] 58300
```

```r
str_length(str_extract_all(new, "(?i)lord"))
```

```
## Warning in stri_extract_all_regex(string, pattern, simplify = simplify, :
## argument is not an atomic vector; coercing

## Warning in stri_extract_all_regex(string, pattern, simplify = simplify, :
## argument is not an atomic vector; coercing
```

```
## [1] 5900
```

```r
wordcount <- bib1 %>%
  mutate(word_count =
           sapply(bib1$scripture_text, function(x) length(unlist(strsplit(as.character(x), "\\W+"))))
  )
```


```r
new1 <- wordcount %>%
  subset(volume_id == 2)
mean(new1$word_count)
```

```
## [1] 22.71019
```


```r
ggplot(data = new1, aes(x = verse_number, y = word_count, fill = book_title)) +
  geom_col() +
  facet_wrap(~book_title, scales = 'free') +
  theme(legend.position = "none") +
  labs(x = "Verse Number",
       y = "Word Count",
       title = "New Testament Average Word by Verse")
```

![](task14_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
This was certanly a hard code for me to do. So to answer the first question what is the average verse length, I made a function that counted the amount of verses in the old and new testement. For the secound question I used the string length function that we learned recently, and counted all the times lord was used between the old and new testiment. My hypothisis was that Lord was used a lot more in the old than the new, because Jesus was in the new testement. That seemed to be true. Finally I made a graph of the word average by verse in the new. This took some word but I jsut got the word count for the new testement and did it by the verse length. 

