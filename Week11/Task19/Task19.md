---
title: "Task19"
author: "Tyler"
date: "11/14/2022"
output: 
  html_document:
   code_folding: "hide"
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
library(sf)
```

```
## Warning: package 'sf' was built under R version 4.1.3
```

```
## Linking to GEOS 3.10.2, GDAL 3.4.1, PROJ 7.2.1; sf_use_s2() is TRUE
```

```r
library(USAboundaries)
library(ggplot2)
library(readr)
library(knitr)
library(tidyverse)
library(dplyr)
library(forcats)
library(downloader)
```

```
## Warning: package 'downloader' was built under R version 4.1.3
```

```r
library(corrplot)
```

```
## Warning: package 'corrplot' was built under R version 4.1.3
```

```
## corrplot 0.92 loaded
```

```r
library(ggrepel)
```

```
## Warning: package 'ggrepel' was built under R version 4.1.3
```

```r
library(sf)
library(maps)
```

```
## Warning: package 'maps' was built under R version 4.1.3
```

```
## 
## Attaching package: 'maps'
## 
## The following object is masked from 'package:purrr':
## 
##     map
```

```r
library(readr)
library(remotes)
```

```
## Warning: package 'remotes' was built under R version 4.1.3
```

```r
library(dygraphs)
```

```
## Warning: package 'dygraphs' was built under R version 4.1.3
```

```r
library(ggsflabel)
```

```
## 
## Attaching package: 'ggsflabel'
## 
## The following objects are masked from 'package:ggplot2':
## 
##     geom_sf_label, geom_sf_text, StatSfCoordinates
```

```r
library(plotly)
```

```
## 
## Attaching package: 'plotly'
## 
## The following object is masked from 'package:ggplot2':
## 
##     last_plot
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following object is masked from 'package:graphics':
## 
##     layout
```

```r
library(gridExtra)
```

```
## Warning: package 'gridExtra' was built under R version 4.1.3
```

```
## 
## Attaching package: 'gridExtra'
## 
## The following object is masked from 'package:dplyr':
## 
##     combine
```

```r
library(leaflet)
```

```
## Warning: package 'leaflet' was built under R version 4.1.3
```


```r
id <- us_counties(states = "ID")
states <- sf::st_as_sf(map("state", plot = FALSE, fill = TRUE))
```



```r
cities <- us_cities()
```

```
## City populations for contemporary data come from the 2010 census.
```



```r
topcity <- cities %>%
  filter(state_name != 'Alaska', state_name != 'Hawaii') %>%
  group_by(state_name) %>%
  arrange(desc(population)) %>%
  slice(1:3)
head(topcity)
```

```
## Simple feature collection with 6 features and 12 fields
## Geometry type: POINT
## Dimension:     XY
## Bounding box:  xmin: -112.088 ymin: 30.66843 xmax: -86.26859 ymax: 33.57216
## Geodetic CRS:  WGS 84
## # A tibble: 6 x 13
## # Groups:   state_name [2]
##   city    state~1 state~2 county count~3 stplf~4 name_~5 city_~6 popul~7 place~8
##   <chr>   <chr>   <chr>   <chr>  <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
## 1 Birmin~ Alabama AL      JEFFE~ Jeffer~ 0107000 Birmin~ US Cen~ US Cen~ Incorp~
## 2 Montgo~ Alabama AL      MONTG~ Montgo~ 0151000 Montgo~ US Cen~ US Cen~ Incorp~
## 3 Mobile  Alabama AL      MOBILE Mobile  0150000 Mobile~ US Cen~ US Cen~ Incorp~
## 4 Phoenix Arizona AZ      MARIC~ Marico~ 0455000 Phoeni~ US Cen~ US Cen~ Incorp~
## 5 Tucson  Arizona AZ      PIMA   Pima    0477000 Tucson~ US Cen~ US Cen~ Incorp~
## 6 Mesa    Arizona AZ      MARIC~ Marico~ 0446000 Mesa c~ US Cen~ US Cen~ Incorp~
## # ... with 3 more variables: year <int>, population <int>,
## #   geometry <POINT [°]>, and abbreviated variable names 1: state_name,
## #   2: state_abbr, 3: county_name, 4: stplfips_2010, 5: name_2010,
## #   6: city_source, 7: population_source, 8: place_type
```



```r
number1 <- topcity %>%
  slice(1:1)
head(number1)
```

```
## Simple feature collection with 6 features and 12 fields
## Geometry type: POINT
## Dimension:     XY
## Bounding box:  xmin: -118.4108 ymin: 33.52744 xmax: -73.19573 ymax: 41.18739
## Geodetic CRS:  WGS 84
## # A tibble: 6 x 13
## # Groups:   state_name [6]
##   city    state~1 state~2 county count~3 stplf~4 name_~5 city_~6 popul~7 place~8
##   <chr>   <chr>   <chr>   <chr>  <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
## 1 Birmin~ Alabama AL      JEFFE~ Jeffer~ 0107000 Birmin~ US Cen~ US Cen~ Incorp~
## 2 Phoenix Arizona AZ      MARIC~ Marico~ 0455000 Phoeni~ US Cen~ US Cen~ Incorp~
## 3 Little~ Arkans~ AR      PULAS~ Pulaski 0541000 Little~ US Cen~ US Cen~ Incorp~
## 4 Los An~ Califo~ CA      LOS A~ Los An~ 0644000 Los An~ US Cen~ US Cen~ Incorp~
## 5 Denver  Colora~ CO      ARAPA~ Arapah~ 0820000 Denver~ US Cen~ US Cen~ Incorp~
## 6 Bridge~ Connec~ CT      FAIRF~ Fairfi~ 0908000 Bridge~ US Cen~ US Cen~ Incorp~
## # ... with 3 more variables: year <int>, population <int>,
## #   geometry <POINT [°]>, and abbreviated variable names 1: state_name,
## #   2: state_abbr, 3: county_name, 4: stplfips_2010, 5: name_2010,
## #   6: city_source, 7: population_source, 8: place_type
```

```r
number2 <- topcity %>%
  slice(2:2)

number3 <- topcity %>%
  slice(3:3)
```




```r
leaflet() %>%
  addTiles() %>%
  addCircleMarkers(data = number1,radius = 7,color = "darkblue",stroke = FALSE,fillOpacity = 1,popup = ~city,label = ~population) %>%
  addCircleMarkers(data = number2,radius = 5,color = "blue",stroke = FALSE,fillOpacity = 1,popup = ~city,label = ~population) %>%
  addCircleMarkers(data = number3,radius = 3,color = "lightblue",stroke = FALSE,fillOpacity = 1,popup = ~city,label = ~population)
```

```{=html}
<div id="htmlwidget-4488c9693a76d6389d59" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-4488c9693a76d6389d59">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"https://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"https://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addCircleMarkers","args":[[33.5274441,33.5721625,34.7254318,34.0193936,39.7618487,41.1873858,39.7352263,38.9041485,30.3370193,33.7629088,43.5984881,41.8375511,39.7766644,41.5739461,37.6906938,38.1780769,30.0686361,43.663811,39.3002135,42.33196,42.3830375,44.9633235,32.3158308,39.125212,45.7895184,41.2646751,36.2277116,42.9846891,40.7242204,35.1055517,40.6642738,35.2087069,46.8652063,39.9847989,35.4670795,45.5369506,40.0093755,41.8230556,34.0297833,43.5383351,35.1035431,29.7804724,40.7785197,44.4919905,36.7793219,47.6204993,38.3486917,43.0633484,41.1521947],[-86.799047,-112.0879662,-92.358556,-118.4108248,-104.8806251,-73.1957339,-75.5292892,-77.0170942,-81.6613021,-84.4226745,-116.2310843,-87.6818441,-86.1459355,-93.616708,-97.3426776,-85.6667077,-89.9390074,-70.255365,-76.6105159,-71.0201725,-83.1022365,-93.2682835,-90.2128226,-94.5511362,-108.5498909,-96.0419269,-115.2640448,-71.4438932,-74.1725735,-106.6473884,-73.9385004,-80.8307389,-96.8290052,-82.9850438,-97.5136565,-122.649971,-75.1333459,-71.4187795,-80.8965664,-96.7319985,-89.9784984,-95.3863425,-111.9314142,-73.2393625,-76.0240202,-122.3508761,-81.632324,-87.9666952,-104.7832381],7,null,null,{"interactive":true,"className":"","stroke":false,"color":"darkblue","weight":5,"opacity":0.5,"fill":true,"fillColor":"darkblue","fillOpacity":1},null,null,["Birmingham","Phoenix","Little Rock","Los Angeles","Denver","Bridgeport","Wilmington","Washington","Jacksonville","Atlanta","Boise","Chicago","Indianapolis","Des Moines","Wichita","Louisville","New Orleans","Portland","Baltimore","Boston","Detroit","Minneapolis","Jackson","Kansas City","Billings","Omaha","Las Vegas","Manchester","Newark","Albuquerque","New York City","Charlotte","Fargo","Columbus","Oklahoma City","Portland","Philadelphia","Providence","Columbia","Sioux Falls","Memphis","Houston","Salt Lake City","Burlington","Virginia Beach","Seattle","Charleston","Milwaukee","Cheyenne"],null,["212237","1445632","193524","3792621","600158","144229","70851","601723","821784","420003","205671","2695598","820445","203433","382368","597337","343829","66194","620961","617594","713777","382578","173514","459787","104170","408958","583756","109565","277140","545852","8175133","731424","105549","787033","579999","583776","1526006","178042","129272","153888","646889","2099451","186440","42417","437994","608660","51400","594833","59466"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addCircleMarkers","args":[[32.3462512,32.1542888,35.3492757,32.8152995,38.8672553,41.3108088,39.1606061,25.775163,33.3655309,43.582381,41.763455,41.0881729,41.9670481,38.8890422,38.0401572,30.4484535,44.0895941,39.4330863,42.2694781,42.961156,44.9488695,30.4160584,38.6356988,46.8693223,40.8089574,36.0122334,42.7490744,40.7114174,32.3264441,42.8924919,35.8302035,46.8110381,41.4781381,36.1279488,44.0567484,40.4397525,41.7007569,32.8179219,44.0710712,36.1718001,29.4724026,40.6884927,44.4364933,36.9230149,47.6735545,38.4106509,43.0878055,42.8405322],[-86.2685927,-110.8710622,-94.370883,-117.134993,-104.760749,-72.924953,-75.5216585,-80.2086152,-82.0734219,-116.5638753,-88.290099,-85.14388,-91.6777599,-94.6905839,-84.4584429,-91.1258993,-70.1721851,-77.4127579,-71.8077831,-85.6555701,-93.1038552,-89.071845,-90.2445816,-114.0103464,-96.6803544,-115.0374619,-71.4905435,-74.0647599,-106.7896951,-78.8596862,-78.6414394,-100.7701017,-81.6794861,-95.9023162,-123.1162068,-79.9765919,-71.4203343,-79.9589267,-103.2179242,-86.7850016,-98.5251419,-112.0117586,-73.1826185,-76.2446413,-117.4165955,-82.4346886,-89.4301208,-106.3201684],5,null,null,{"interactive":true,"className":"","stroke":false,"color":"blue","weight":5,"opacity":0.5,"fill":true,"fillColor":"blue","fillOpacity":1},null,null,["Montgomery","Tucson","Fort Smith","San Diego","Colorado Springs","New Haven","Dover","Miami","Augusta","Nampa","Aurora","Fort Wayne","Cedar Rapids","Overland Park","Lexington","Baton Rouge","Lewiston","Frederick","Worcester","Grand Rapids","St. Paul","Gulfport","St. Louis","Missoula","Lincoln","Henderson","Nashua","Jersey City","Las Cruces","Buffalo","Raleigh","Bismarck","Cleveland","Tulsa","Eugene","Pittsburgh","Warwick","Charleston","Rapid City","Nashville","San Antonio","West Valley City","South Burlington","Norfolk","Spokane","Huntington","Madison","Casper"],null,["205764","520116","86209","1307402","416427","129779","36047","399457","195844","81557","197899","253691","126326","173372","295803","229493","36592","65239","181045","188040","285068","67793","319294","66788","258379","257729","86494","247597","97618","261310","403892","61272","396815","391906","156185","305704","82672","120083","67956","601222","1327407","129480","17904","242803","208916","49138","233209","55316"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addCircleMarkers","args":[[30.668426,33.4019259,36.0715607,37.2968672,39.6880021,41.7660453,39.677569,27.9700861,32.510197,43.617852,42.2633938,37.9877339,41.5541036,39.1225389,36.9709194,32.4670204,44.8306292,39.0836473,42.115454,42.4929044,44.0154424,34.9508416,37.194152,47.5014389,41.1542932,39.4744867,43.2324926,40.9147462,35.2850958,43.1699272,36.0964835,47.9127893,39.1399019,35.2405774,44.9236937,40.5939656,41.7697341,32.9173424,45.4646777,35.9708752,32.794176,40.2453301,43.6096702,36.6793761,47.2521991,39.2613533,44.5206763,41.3102405],[-88.1002261,-111.7173787,-94.1665396,-121.8193058,-104.68974,-72.6833394,-75.7573084,-82.4796734,-84.874946,-116.397129,-89.0628271,-87.534703,-90.6039968,-94.7417813,-86.4384623,-93.7926845,-68.7923966,-77.1556178,-72.5399783,-83.0250007,-92.4772105,-89.977981,-93.2913048,-111.299975,-95.9328384,-119.7765384,-71.5612523,-74.1628255,-106.6988694,-77.6168907,-79.8271079,-97.0750101,-84.5064465,-97.3453056,-123.0231194,-75.4781595,-71.4850489,-80.0651497,-98.468104,-83.9464787,-96.7655033,-111.6448069,-72.9777884,-76.3017884,-122.4598318,-81.5433763,-87.9841686,-105.6096249],3,null,null,{"interactive":true,"className":"","stroke":false,"color":"lightblue","weight":5,"opacity":0.5,"fill":true,"fillColor":"lightblue","fillOpacity":1},null,null,["Mobile","Mesa","Fayetteville","San Jose","Aurora","Hartford","Newark","Tampa","Columbus","Meridian","Rockford","Evansville","Davenport","Kansas City","Bowling Green","Shreveport","Bangor","Rockville","Springfield","Warren","Rochester","Southaven","Springfield","Great Falls","Bellevue","Reno","Concord","Paterson","Rio Rancho","Rochester","Greensboro","Grand Forks","Cincinnati","Norman","Salem","Allentown","Cranston","North Charleston","Aberdeen","Knoxville","Dallas","Provo","Rutland","Chesapeake","Tacoma","Parkersburg","Green Bay","Laramie"],null,["195111","439041","73580","945942","325078","124775","31454","335709","189885","75092","152871","117429","99685","145786","58067","199311","33039","61209","153060","134056","106769","48982","159498","58505","50137","225221","42695","146199","87521","210565","269666","52838","296943","110925","154637","118032","80387","97471","26091","178874","1197816","112488","16495","222209","198397","31492","104057","30816"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]}],"limits":{"lat":[25.775163,47.9127893],"lng":[-123.1162068,-68.7923966]}},"evals":[],"jsHooks":[]}</script>
```

