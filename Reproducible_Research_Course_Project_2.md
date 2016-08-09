Reproducible Research Course Project 2
================
TK
9 août 2016

``` r
library(rmarkdown)
```

    ## Warning: package 'rmarkdown' was built under R version 3.2.5

``` r
library(knitr)
Sys.setlocale("LC_TIME", "English")
```

    ## [1] "English_United States.1252"

``` r
setwd("E:/Coursera/Reproducible Research/Week 4/Course Project 2")
```

Introduction
------------

Storms and other severe weather events can cause both public health and economic problems for communities and municipalities. Many severe events can result in fatalities, injuries, and property damage, and preventing such outcomes to the extent possible is a key concern.

This project involves exploring the U.S. National Oceanic and Atmospheric Administration's (NOAA) storm database. This database tracks characteristics of major storms and weather events in the United States, including when and where they occur, as well as estimates of any fatalities, injuries, and property damage.

Data
----

The data for this assignment come in the form of a comma-separated-value file compressed via the bzip2 algorithm to reduce its size. You can download the file from the course web site:

Storm Data \[47Mb\] There is also some documentation of the database available. Here you will find how some of the variables are constructed/defined.

National Weather Service Storm Data Documentation National Climatic Data Center Storm Events FAQ The events in the database start in the year 1950 and end in November 2011. In the earlier years of the database there are generally fewer events recorded, most likely due to a lack of good records. More recent years should be considered more complete.

1.Download the file and put the file in the data folder
-------------------------------------------------------

``` r
if(!file.exists("./data")){dir.create("./data")}
fileUrl <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2"
download.file(fileUrl,destfile="./data/StormData.csv.bz2")
```

2.Read the file
---------------

``` r
Stormdata<-read.csv("StormData.csv", header=TRUE, stringsAsFactors=FALSE) 

str(Stormdata)
```

    ## 'data.frame':    902297 obs. of  37 variables:
    ##  $ STATE__   : num  1 1 1 1 1 1 1 1 1 1 ...
    ##  $ BGN_DATE  : chr  "4/18/1950 0:00:00" "4/18/1950 0:00:00" "2/20/1951 0:00:00" "6/8/1951 0:00:00" ...
    ##  $ BGN_TIME  : chr  "0130" "0145" "1600" "0900" ...
    ##  $ TIME_ZONE : chr  "CST" "CST" "CST" "CST" ...
    ##  $ COUNTY    : num  97 3 57 89 43 77 9 123 125 57 ...
    ##  $ COUNTYNAME: chr  "MOBILE" "BALDWIN" "FAYETTE" "MADISON" ...
    ##  $ STATE     : chr  "AL" "AL" "AL" "AL" ...
    ##  $ EVTYPE    : chr  "TORNADO" "TORNADO" "TORNADO" "TORNADO" ...
    ##  $ BGN_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ BGN_AZI   : chr  "" "" "" "" ...
    ##  $ BGN_LOCATI: chr  "" "" "" "" ...
    ##  $ END_DATE  : chr  "" "" "" "" ...
    ##  $ END_TIME  : chr  "" "" "" "" ...
    ##  $ COUNTY_END: num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ COUNTYENDN: logi  NA NA NA NA NA NA ...
    ##  $ END_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ END_AZI   : chr  "" "" "" "" ...
    ##  $ END_LOCATI: chr  "" "" "" "" ...
    ##  $ LENGTH    : num  14 2 0.1 0 0 1.5 1.5 0 3.3 2.3 ...
    ##  $ WIDTH     : num  100 150 123 100 150 177 33 33 100 100 ...
    ##  $ F         : int  3 2 2 2 2 2 2 1 3 3 ...
    ##  $ MAG       : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ FATALITIES: num  0 0 0 0 0 0 0 0 1 0 ...
    ##  $ INJURIES  : num  15 0 2 2 2 6 1 0 14 0 ...
    ##  $ PROPDMG   : num  25 2.5 25 2.5 2.5 2.5 2.5 2.5 25 25 ...
    ##  $ PROPDMGEXP: chr  "K" "K" "K" "K" ...
    ##  $ CROPDMG   : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ CROPDMGEXP: chr  "" "" "" "" ...
    ##  $ WFO       : chr  "" "" "" "" ...
    ##  $ STATEOFFIC: chr  "" "" "" "" ...
    ##  $ ZONENAMES : chr  "" "" "" "" ...
    ##  $ LATITUDE  : num  3040 3042 3340 3458 3412 ...
    ##  $ LONGITUDE : num  8812 8755 8742 8626 8642 ...
    ##  $ LATITUDE_E: num  3051 0 0 0 0 ...
    ##  $ LONGITUDE_: num  8806 0 0 0 0 ...
    ##  $ REMARKS   : chr  "" "" "" "" ...
    ##  $ REFNUM    : num  1 2 3 4 5 6 7 8 9 10 ...

``` r
dim(Stormdata)
```

    ## [1] 902297     37

``` r
names(Stormdata)
```

    ##  [1] "STATE__"    "BGN_DATE"   "BGN_TIME"   "TIME_ZONE"  "COUNTY"    
    ##  [6] "COUNTYNAME" "STATE"      "EVTYPE"     "BGN_RANGE"  "BGN_AZI"   
    ## [11] "BGN_LOCATI" "END_DATE"   "END_TIME"   "COUNTY_END" "COUNTYENDN"
    ## [16] "END_RANGE"  "END_AZI"    "END_LOCATI" "LENGTH"     "WIDTH"     
    ## [21] "F"          "MAG"        "FATALITIES" "INJURIES"   "PROPDMG"   
    ## [26] "PROPDMGEXP" "CROPDMG"    "CROPDMGEXP" "WFO"        "STATEOFFIC"
    ## [31] "ZONENAMES"  "LATITUDE"   "LONGITUDE"  "LATITUDE_E" "LONGITUDE_"
    ## [36] "REMARKS"    "REFNUM"

3. Selecting Variables
----------------------

The dataset has 37 variables. For this analysis we need seven of these variables. These are: EVTYPE, FATALITIES, INJURIES, PROPDMG, PROPDMGEXP, CROPDMG, CROPDMGEXP.

We selecting this variables.

``` r
variables<-c("EVTYPE","FATALITIES","INJURIES","PROPDMG", "PROPDMGEXP","CROPDMG","CROPDMGEXP")
data<-Stormdata[variables]

dim(data)
```

    ## [1] 902297      7

``` r
names(data)
```

    ## [1] "EVTYPE"     "FATALITIES" "INJURIES"   "PROPDMG"    "PROPDMGEXP"
    ## [6] "CROPDMG"    "CROPDMGEXP"

4. Selecting events that cause the most fatalities and injuries.
----------------------------------------------------------------

``` r
fatal <- aggregate(FATALITIES ~ EVTYPE, data = data, sum)
fatal <- fatal [order(fatal$FATALITIES, decreasing=TRUE),]
head(fatal)
```

    ##             EVTYPE FATALITIES
    ## 834        TORNADO       5633
    ## 130 EXCESSIVE HEAT       1903
    ## 153    FLASH FLOOD        978
    ## 275           HEAT        937
    ## 464      LIGHTNING        816
    ## 856      TSTM WIND        504

5. Plot Fatalities
------------------

``` r
par(mar=c(12, 6, 1, 1))
barplot (height = fatal$FATALITIES[1:10], names.arg = fatal$EVTYPE[1:10], las = 2, cex.names= 0.8,
         col = rainbow (10, start=0, end=0.5))
title (main = "Fatalities: Top 10 Events", line=-5)
title (ylab = "Total number of Fatalities", line=4)
```

![](https://github.com/xetaro/Reproducible-Research-Course-Project-2/blob/master/plot_1.png)

6. Selecting events that cause the most injuries.
-------------------------------------------------

``` r
injury <- aggregate(INJURIES ~ EVTYPE, data = data, sum)
injury <- injury[order(injury$INJURIES, decreasing=TRUE),]
head(injury)
```

    ##             EVTYPE INJURIES
    ## 834        TORNADO    91346
    ## 856      TSTM WIND     6957
    ## 170          FLOOD     6789
    ## 130 EXCESSIVE HEAT     6525
    ## 464      LIGHTNING     5230
    ## 275           HEAT     2100

7. Plot Injuries
----------------

``` r
par(mar=c(12, 6, 1, 1))
barplot (height = injury$INJURIES[1:10], names.arg = injury$EVTYPE[1:10], las = 2, cex.names = 0.8,
         col = rainbow (10, start=0, end=0.5))
title (main = "Injuries: Top 10 Events", line=-5)
title (ylab = "Total number of Injuries", line=4)
```

![](Reproducible_Research_Course_Project_2_files/figure-markdown_github/unnamed-chunk-8-1.png)

8. Total economic damage produced by each type of event.
--------------------------------------------------------

Property and crop damages are summed-up over the years from 1950.

Finding property damage
-----------------------

Property damage exponents for each level was listed out and assigned those values for the property exponent data. Invalid data was excluded by assigning the value as '0'. Then property damage value was calculated by multiplying the property damage and property exponent value.

``` r
unique(data$PROPDMGEXP)
```

    ##  [1] "K" "M" ""  "B" "m" "+" "0" "5" "6" "?" "4" "2" "3" "h" "7" "H" "-"
    ## [18] "1" "8"

``` r
# Assigning values for the property exponent data 
data$PROPEXP[data$PROPDMGEXP == "K"] <- 1000
data$PROPEXP[data$PROPDMGEXP == "M"] <- 1e+06
data$PROPEXP[data$PROPDMGEXP == ""] <- 1
data$PROPEXP[data$PROPDMGEXP == "B"] <- 1e+09
data$PROPEXP[data$PROPDMGEXP == "m"] <- 1e+06
data$PROPEXP[data$PROPDMGEXP == "0"] <- 1
data$PROPEXP[data$PROPDMGEXP == "5"] <- 1e+05
data$PROPEXP[data$PROPDMGEXP == "6"] <- 1e+06
data$PROPEXP[data$PROPDMGEXP == "4"] <- 10000
data$PROPEXP[data$PROPDMGEXP == "2"] <- 100
data$PROPEXP[data$PROPDMGEXP == "3"] <- 1000
data$PROPEXP[data$PROPDMGEXP == "h"] <- 100
data$PROPEXP[data$PROPDMGEXP == "7"] <- 1e+07
data$PROPEXP[data$PROPDMGEXP == "H"] <- 100
data$PROPEXP[data$PROPDMGEXP == "1"] <- 10
data$PROPEXP[data$PROPDMGEXP == "8"] <- 1e+08

# Assigning '0' to invalid exponent data
data$PROPEXP[data$PROPDMGEXP == "+"] <- 0
data$PROPEXP[data$PROPDMGEXP == "-"] <- 0
data$PROPEXP[data$PROPDMGEXP == "?"] <- 0

# Calculating the property damage value
data$PROPDMGVAL <- data$PROPDMG * data$PROPEXP
data$PROPDMGVAL <-as.numeric(data$PROPDMGVAL)
```

Finding crop damage
-------------------

Crop damage exponents for each level was listed out and assigned those values for the crop exponent data. Invalid data was excluded by assigning the value as '0'. Then crop damage value was calculated by multiplying the crop damage and crop exponent value.

``` r
unique(data$CROPDMGEXP)
```

    ## [1] ""  "M" "K" "m" "B" "?" "0" "k" "2"

``` r
# Assigning values for the crop exponent data 
data$CROPEXP[data$CROPDMGEXP == "M"] <- 1e+06
data$CROPEXP[data$CROPDMGEXP == "K"] <- 1000
data$CROPEXP[data$CROPDMGEXP == "m"] <- 1e+06
data$CROPEXP[data$CROPDMGEXP == "B"] <- 1e+09
data$CROPEXP[data$CROPDMGEXP == "0"] <- 1
data$CROPEXP[data$CROPDMGEXP == "k"] <- 1000
data$CROPEXP[data$CROPDMGEXP == "2"] <- 100
data$CROPEXP[data$CROPDMGEXP == ""] <- 1

# Assigning '0' to invalid exponent data
data$CROPEXP[data$CROPDMGEXP == "?"] <- 0


# calculating the crop damage 
data$CROPDMGVAL <- data$CROPDMG * data$CROPEXP
data$CROPDMGVAL <-as.numeric(data$CROPDMGVAL)
```

Total damages was estimated:
----------------------------

``` r
data$dmgtot <- data$CROPDMGVAL+data$PROPDMGVAL
dmgtot <- aggregate(dmgtot ~ EVTYPE, data, FUN = sum)
dmgtot <- dmgtot[order(dmgtot$dmgtot, decreasing=TRUE),]
head(dmgtot)
```

    ##                EVTYPE       dmgtot
    ## 170             FLOOD 150319678257
    ## 411 HURRICANE/TYPHOON  71913712800
    ## 834           TORNADO  57362333887
    ## 670       STORM SURGE  43323541000
    ## 244              HAIL  18761221986
    ## 153       FLASH FLOOD  18243991079

``` r
barplot (height = dmgtot$dmgtot[1:10]/(10^9), names.arg = dmgtot$EVTYPE[1:10], las = 2, cex.names= 0.6,
         col = rainbow (10, start=0, end=0.5))
title (main = "Total Damages", line=-1)
title (ylab = "Damage Cost ($ billions)", line=3)
```

![](Reproducible_Research_Course_Project_2_files/figure-markdown_github/unnamed-chunk-11-1.png)

Results and Conclusions
-----------------------

Tornados caused the maximum number of fatalities and injuries.

It was followed by Excessive Heat for fatalities and Thunderstorm wind for injuries.

Floods and Hurricanes/Typhoos caused the maximum damage for total damages (property and crop damage).
