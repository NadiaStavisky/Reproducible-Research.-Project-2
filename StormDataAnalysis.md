---
title: "Reproducible Research. Project 2"
author: "Nadia Stavisky"
date: "7/28/2019"
output: 
  html_document:
    keep_md: true

---



### The weather events analysis of impact on US population health and economic.

##Introduction
Storms and other severe weather events can cause both public health and economic problems for communities and municipalities. Many severe events can result in fatalities, injuries, and property damage, and preventing such outcomes to the extent possible is a key concern.

This project involves exploring the U.S. National Oceanic and Atmospheric Administration's (NOAA) storm database. This database tracks characteristics of major storms and weather events in the United States, including when and where they occur, as well as estimates of any fatalities, injuries, and property damage.

The events in the database start in the year 1950 and end in November 2011. In the earlier years of the database there are generally fewer events recorded, most likely due to a lack of good records. More recent years should be considered more complete.

##Data Processing

The Data for this analysis loaded from [Storm Data](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2)


```r
# loading data using the link, reading data into 'data' - R
# object:
datapath <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2"
download.file(datapath, destfile = "./StormData.csv.bz2")
data <- read.csv2("StormData.csv.bz2", header = TRUE, sep = ",")
```

data dimensions and structure:

```r
dim(data)
```

```
## [1] 902297     37
```

```r
str(data)
```

```
## 'data.frame':	902297 obs. of  37 variables:
##  $ STATE__   : Factor w/ 70 levels "1.00","10.00",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ BGN_DATE  : Factor w/ 16335 levels "1/1/1966 0:00:00",..: 6523 6523 4242 11116 2224 2224 2260 383 3980 3980 ...
##  $ BGN_TIME  : Factor w/ 3608 levels "00:00:00 AM",..: 272 287 2705 1683 2584 3186 242 1683 3186 3186 ...
##  $ TIME_ZONE : Factor w/ 22 levels "ADT","AKS","AST",..: 7 7 7 7 7 7 7 7 7 7 ...
##  $ COUNTY    : Factor w/ 557 levels "0.00","1.00",..: 555 208 423 546 307 507 547 29 31 423 ...
##  $ COUNTYNAME: Factor w/ 29601 levels "","5NM E OF MACKINAC BRIDGE TO PRESQUE ISLE LT MI",..: 13513 1873 4598 10592 4372 10094 1973 23873 24418 4598 ...
##  $ STATE     : Factor w/ 72 levels "AK","AL","AM",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ EVTYPE    : Factor w/ 985 levels "   HIGH SURF ADVISORY",..: 834 834 834 834 834 834 834 834 834 834 ...
##  $ BGN_RANGE : Factor w/ 272 levels "0.00","0.10",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ BGN_AZI   : Factor w/ 35 levels "","  N"," NW",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ BGN_LOCATI: Factor w/ 54429 levels ""," Christiansburg",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_DATE  : Factor w/ 6663 levels "","1/1/1993 0:00:00",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_TIME  : Factor w/ 3647 levels ""," 0900CST",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ COUNTY_END: Factor w/ 1 level "0.00": 1 1 1 1 1 1 1 1 1 1 ...
##  $ COUNTYENDN: logi  NA NA NA NA NA NA ...
##  $ END_RANGE : Factor w/ 266 levels "0.00","0.10",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_AZI   : Factor w/ 24 levels "","E","ENE","ESE",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_LOCATI: Factor w/ 34506 levels ""," CANTON"," TULIA",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ LENGTH    : Factor w/ 568 levels "0.00","0.10",..: 74 138 2 1 1 22 22 1 254 141 ...
##  $ WIDTH     : Factor w/ 293 levels "0.00","1.00",..: 4 62 29 4 62 86 164 164 4 4 ...
##  $ F         : int  3 2 2 2 2 2 2 1 3 3 ...
##  $ MAG       : Factor w/ 226 levels "0.00","1.00",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ FATALITIES: Factor w/ 52 levels "0.00","1.00",..: 1 1 1 1 1 1 1 1 2 1 ...
##  $ INJURIES  : Factor w/ 200 levels "0.00","1.00",..: 42 1 69 69 69 154 2 1 36 1 ...
##  $ PROPDMG   : Factor w/ 1390 levels "0.00","0.01",..: 559 446 559 446 446 446 446 446 559 559 ...
##  $ PROPDMGEXP: Factor w/ 19 levels "","-","?","+",..: 17 17 17 17 17 17 17 17 17 17 ...
##  $ CROPDMG   : Factor w/ 432 levels "0.00","0.01",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ CROPDMGEXP: Factor w/ 9 levels "","?","0","2",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ WFO       : Factor w/ 542 levels ""," CI","%SD",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ STATEOFFIC: Factor w/ 250 levels "","ALABAMA, Central",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ ZONENAMES : Factor w/ 25112 levels "","                                                                                                               "| __truncated__,..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ LATITUDE  : Factor w/ 1781 levels "","0.00","1328.00",..: 558 560 738 816 770 808 763 693 732 734 ...
##  $ LONGITUDE : Factor w/ 3841 levels "-14445.00","-14451.00",..: 3133 3116 3103 3027 3043 3109 3032 2999 3101 3099 ...
##  $ LATITUDE_E: Factor w/ 1729 levels "","0.00","1334.00",..: 533 2 2 2 2 2 2 2 698 699 ...
##  $ LONGITUDE_: Factor w/ 3778 levels "-14455.00","0.00",..: 3065 2 2 2 2 2 2 2 3037 3036 ...
##  $ REMARKS   : Factor w/ 436781 levels "","\t","\t\t",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ REFNUM    : Factor w/ 902297 levels "1.00","10.00",..: 1 111112 222223 333334 444445 555556 666667 777778 888889 2 ...
```

```r
summary(data)
```

```
##     STATE__                    BGN_DATE             BGN_TIME     
##  48.00  : 83728   5/25/2011 0:00:00:  1202   12:00:00 AM: 10163  
##  20.00  : 53441   4/27/2011 0:00:00:  1193   06:00:00 PM:  7350  
##  40.00  : 46802   6/9/2011 0:00:00 :  1030   04:00:00 PM:  7261  
##  29.00  : 35648   5/30/2004 0:00:00:  1016   05:00:00 PM:  6891  
##  19.00  : 31069   4/4/2011 0:00:00 :  1009   12:00:00 PM:  6703  
##  31.00  : 30271   4/2/2006 0:00:00 :   981   03:00:00 PM:  6700  
##  (Other):621338   (Other)          :895866   (Other)    :857229  
##    TIME_ZONE          COUNTY            COUNTYNAME         STATE       
##  CST    :547493   1.00   : 17810   JEFFERSON :  7840   TX     : 83728  
##  EST    :245558   3.00   : 16220   WASHINGTON:  7603   KS     : 53440  
##  MST    : 68390   19.00  : 14141   JACKSON   :  6660   OK     : 46802  
##  PST    : 28302   15.00  : 13858   FRANKLIN  :  6256   MO     : 35648  
##  AST    :  6360   5.00   : 13847   LINCOLN   :  5937   IA     : 31069  
##  HST    :  2563   17.00  : 13081   MADISON   :  5632   NE     : 30271  
##  (Other):  3631   (Other):813340   (Other)   :862369   (Other):621339  
##                EVTYPE         BGN_RANGE         BGN_AZI      
##  HAIL             :288661   0.00   :607811          :547332  
##  TSTM WIND        :219940   1.00   : 80016   N      : 86752  
##  THUNDERSTORM WIND: 82563   2.00   : 49981   W      : 38446  
##  TORNADO          : 60652   3.00   : 36214   S      : 37558  
##  FLASH FLOOD      : 54277   5.00   : 26473   E      : 33178  
##  FLOOD            : 25326   4.00   : 24373   NW     : 24041  
##  (Other)          :170878   (Other): 77429   (Other):134990  
##          BGN_LOCATI                  END_DATE             END_TIME     
##               :287743                    :243411              :238978  
##  COUNTYWIDE   : 19680   4/27/2011 0:00:00:  1214   06:00:00 PM:  9802  
##  Countywide   :   993   5/25/2011 0:00:00:  1196   05:00:00 PM:  8314  
##  SPRINGFIELD  :   843   6/9/2011 0:00:00 :  1021   04:00:00 PM:  8104  
##  SOUTH PORTION:   810   4/4/2011 0:00:00 :  1007   12:00:00 PM:  7483  
##  NORTH PORTION:   784   5/30/2004 0:00:00:   998   11:59:00 PM:  7184  
##  (Other)      :591444   (Other)          :653450   (Other)    :622432  
##  COUNTY_END    COUNTYENDN       END_RANGE         END_AZI      
##  0.00:902297   Mode:logical   0.00   :733369          :724837  
##                NA's:902297    1.00   : 29582   N      : 28082  
##                               2.00   : 27938   S      : 22510  
##                               3.00   : 21767   W      : 20119  
##                               5.00   : 18765   E      : 20047  
##                               4.00   : 15080   NE     : 14606  
##                               (Other): 55796   (Other): 72096  
##            END_LOCATI         LENGTH           WIDTH       
##                 :499225   0.00   :850839   0.00   :841747  
##  COUNTYWIDE     : 19731   0.10   :  7699   33.00  : 10157  
##  SOUTH PORTION  :   833   1.00   :  6520   50.00  :  8258  
##  NORTH PORTION  :   780   0.50   :  4581   100.00 :  6187  
##  CENTRAL PORTION:   617   0.20   :  4131   10.00  :  4646  
##  SPRINGFIELD    :   575   2.00   :  3928   20.00  :  3687  
##  (Other)        :380536   (Other): 24599   (Other): 27615  
##        F               MAG           FATALITIES        INJURIES     
##  Min.   :0.0      0.00   :366676   0.00   :895323   0.00   :884693  
##  1st Qu.:0.0      75.00  : 84595   1.00   :  5010   1.00   :  7756  
##  Median :1.0      50.00  : 71880   2.00   :   996   2.00   :  3134  
##  Mean   :0.9      100.00 : 55400   3.00   :   314   3.00   :  1552  
##  3rd Qu.:1.0      52.00  : 47411   4.00   :   166   4.00   :   931  
##  Max.   :5.0      175.00 : 46645   5.00   :   114   5.00   :   709  
##  NA's   :843563   (Other):229690   (Other):   374   (Other):  3522  
##     PROPDMG         PROPDMGEXP        CROPDMG         CROPDMGEXP    
##  0.00   :663123          :465934   0.00   :880198          :618413  
##  5.00   : 32655   K      :424665   5.00   :  4276   K      :281832  
##  10.00  : 22018   M      : 11330   10.00  :  2381   M      :  1994  
##  1.00   : 19069   0      :   216   50.00  :  2011   k      :    21  
##  2.00   : 17872   B      :    40   1.00   :  1404   0      :    19  
##  25.00  : 17696   5      :    28   100.00 :  1237   B      :     9  
##  (Other):129864   (Other):    84   (Other): 10790   (Other):     9  
##       WFO                                       STATEOFFIC    
##         :142069                                      :248769  
##  OUN    : 17393   TEXAS, North                       : 12193  
##  JAN    : 13889   ARKANSAS, Central and North Central: 11738  
##  LWX    : 13174   IOWA, Central                      : 11345  
##  PHI    : 12551   KANSAS, Southwest                  : 11212  
##  TSA    : 12483   GEORGIA, North and Central         : 11120  
##  (Other):690738   (Other)                            :595920  
##                                                                                                                                                                                                     ZONENAMES     
##                                                                                                                                                                                                          :594029  
##                                                                                                                                                                                                          :205988  
##  GREATER RENO / CARSON CITY / M - GREATER RENO / CARSON CITY / M                                                                                                                                         :   639  
##  GREATER LAKE TAHOE AREA - GREATER LAKE TAHOE AREA                                                                                                                                                       :   592  
##  JEFFERSON - JEFFERSON                                                                                                                                                                                   :   303  
##  MADISON - MADISON                                                                                                                                                                                       :   302  
##  (Other)                                                                                                                                                                                                 :100444  
##     LATITUDE        LONGITUDE        LATITUDE_E       LONGITUDE_    
##  0.00   :214172   0.00   :214173   0.00   :554953   0.00   :554953  
##  3512.00:  1640   9724.00:  1202   4140.00:   701   9720.00:   511  
##  4140.00:  1357   9735.00:  1062   3609.00:   668   9732.00:   481  
##  3430.00:  1355   9830.00:   997   3858.00:   667   9422.00:   474  
##  3513.00:  1346   9707.00:   959   3512.00:   663   9707.00:   473  
##  3742.00:  1317   9736.00:   911   3513.00:   660   9557.00:   463  
##  (Other):681110   (Other):682993   (Other):343985   (Other):344942  
##                                            REMARKS      
##                                                :287433  
##                                                : 24013  
##  Trees down.\n                                 :  1110  
##  Several trees were blown down.\n              :   568  
##  Trees were downed.\n                          :   446  
##  Large trees and power lines were blown down.\n:   432  
##  (Other)                                       :588295  
##        REFNUM      
##  1.00     :     1  
##  10.00    :     1  
##  100.00   :     1  
##  1000.00  :     1  
##  10000.00 :     1  
##  100000.00:     1  
##  (Other)  :902291
```

Subsetting data, selecting columns that are contain information:
1. EVTYPE - event type (factor);
2. FATALITIES - number of recorded fatalities;
3. INJURIES - number of recorded injuries;
4. PROPDMG - Property damage estimates;
5. PROPDMGEXP - magnitude of PROPDMG ( “K” for thousands, “M” for millions, and “B” for billions etc.);
6. CROPDMG - crop damage estimates;
7. CROPDMGEXP - magnitude of CROPDMG ( “K” for thousands, “M” for millions, and “B” for billions etc.);



```r
subsetdata <- data[, c("BGN_DATE", "REFNUM", "EVTYPE", "FATALITIES", 
    "INJURIES", "PROPDMG", "PROPDMGEXP", "CROPDMG", "CROPDMGEXP")]
```
working dataset:

```r
data1 <- subsetdata
```


```r
head(data1)
```

```
##             BGN_DATE REFNUM  EVTYPE FATALITIES INJURIES PROPDMG PROPDMGEXP
## 1  4/18/1950 0:00:00   1.00 TORNADO       0.00    15.00   25.00          K
## 2  4/18/1950 0:00:00   2.00 TORNADO       0.00     0.00    2.50          K
## 3  2/20/1951 0:00:00   3.00 TORNADO       0.00     2.00   25.00          K
## 4   6/8/1951 0:00:00   4.00 TORNADO       0.00     2.00    2.50          K
## 5 11/15/1951 0:00:00   5.00 TORNADO       0.00     2.00    2.50          K
## 6 11/15/1951 0:00:00   6.00 TORNADO       0.00     6.00    2.50          K
##   CROPDMG CROPDMGEXP
## 1    0.00           
## 2    0.00           
## 3    0.00           
## 4    0.00           
## 5    0.00           
## 6    0.00
```
Data transformation:

transform "FATALITIES", "INJURIES", "PROPDMG", "CROPDMG" from factors in to numeric:

```r
numcol <- c("FATALITIES", "INJURIES", "PROPDMG", "CROPDMG")
data1[, numcol] <- mutate_if(data1[, numcol], is.factor, as.character)
data1[, numcol] <- sapply(data1[, numcol], as.numeric)
```
transform BGN_DATE from factors to dates

```r
data1$BGN_DATE <- mdy_hms(data1$BGN_DATE)
data1$BGN_DATE <- year(data1$BGN_DATE)
```

Standartize magnitude codes used in the dataset:

```r
levels(data1$PROPDMGEXP)
```

```
##  [1] ""  "-" "?" "+" "0" "1" "2" "3" "4" "5" "6" "7" "8" "B" "h" "H" "K"
## [18] "m" "M"
```

```r
levels(data1$CROPDMGEXP)
```

```
## [1] ""  "?" "0" "2" "B" "k" "K" "m" "M"
```


```r
# create df for PROPDMGEXP$CROPDMGEXP recoding:
DMGEXPlevels <- unique(as.character(c(levels(data1$PROPDMGEXP), 
    levels(data1$CROPDMGEXP))))
DMGEXPint <- c(1, 1, 1, 1, 1, 1, 10^2, 10^3, 10^4, 10^5, 10^6, 
    10^7, 10^8, 10^9, 10^2, 10^2, 10^3, 10^6, 10^6, 10^3)
PMGTDdf <- cbind(PROPDMGEXP = DMGEXPlevels, PDMGEXPint = DMGEXPint)
CMGTDdf <- cbind(CROPDMGEXP = DMGEXPlevels, CDMGEXPint = DMGEXPint)
```


```r
# create data set to use in visual analysis
data2 <- merge(data1, PMGTDdf, by = "PROPDMGEXP", all.x = TRUE)
data2 <- merge(data2, CMGTDdf, by = "CROPDMGEXP", all.x = TRUE)
data2$PROPDMGB <- (data2$PROPDMG * as.numeric(as.character(data2$PDMGEXPint)))/1e+09
data2$CROPDMGB <- (data2$CROPDMG * as.numeric(as.character(data2$CDMGEXPint)))/1e+09
data2$DMGEXPB <- data2$PROPDMGB + data2$CROPDMGB
data2$TTLHARM <- data2$FATALITIES + data2$INJURIES
```

```r
str(data2)
```

```
## 'data.frame':	902297 obs. of  15 variables:
##  $ CROPDMGEXP: Factor w/ 9 levels "","?","0","2",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ PROPDMGEXP: Factor w/ 19 levels "","-","?","+",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ BGN_DATE  : num  1991 1992 1959 1983 1990 ...
##  $ REFNUM    : Factor w/ 902297 levels "1.00","10.00",..: 21673 398857 12791 544647 409224 14389 884846 669969 325936 329757 ...
##  $ EVTYPE    : Factor w/ 985 levels "   HIGH SURF ADVISORY",..: 856 856 856 856 856 244 856 856 856 856 ...
##  $ FATALITIES: num  0 0 0 0 0 0 0 0 0 0 ...
##  $ INJURIES  : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ PROPDMG   : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ CROPDMG   : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ PDMGEXPint: Factor w/ 9 levels "1","100","1000",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ CDMGEXPint: Factor w/ 9 levels "1","100","1000",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ PROPDMGB  : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ CROPDMGB  : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ DMGEXPB   : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ TTLHARM   : num  0 0 0 0 0 0 0 0 0 0 ...
```
##Results

#Top weather events associated with greatest health harm.

Weather events grouped as:
- Fatalities;
- Injuries;
- Fatalities + Injuries = Total Health Harm;

```r
healthplot <- data2 %>% subset(select = c(EVTYPE, FATALITIES, 
    INJURIES, TTLHARM)) %>% gather(HealthDMGType, Value, c(FATALITIES, 
    INJURIES, TTLHARM)) %>% group_by(HealthDMGType, EVTYPE) %>% 
    summarize(sum_health_harm_K = sum(Value)/1000) %>% arrange(desc(sum_health_harm_K)) %>% 
    group_by(HealthDMGType) %>% slice(1:10)
```


```r
healthplot$HealthDMGType <- factor(healthplot$HealthDMGType, 
    levels = c("FATALITIES", "INJURIES", "TTLHARM"), labels = c("Fatalities", 
        "Injuries", "Total"))
a <- ggplot(data = healthplot, aes(x = reorder(EVTYPE, -sum_health_harm_K, 
    sum), y = sum_health_harm_K, fill = HealthDMGType))
a + geom_bar(stat = "identity") + facet_grid(HealthDMGType ~ 
    ., scales = "free") + geom_text(aes(label = sum_health_harm_K), 
    size = 3) + theme(axis.text.x = element_text(angle = 45, 
    hjust = 1)) + scale_fill_discrete(name = "Health demadge type", 
    labels = c("Fatalities", "Injuries", "Fatalities + Injuries")) + 
    labs(title = "Total Fatalities & Injuries", x = "", y = "Number of injuries, K")
```

<img src="figs/unnamed-chunk-13-1.png" style="display: block; margin: auto;" />
#Summary. Top 5 weather events cused greatest health harm.

```r
healthplot %>% spread(HealthDMGType, sum_health_harm_K) %>% arrange(desc(Total)) %>% 
    head(5)
```

```
## # A tibble: 5 x 4
##   EVTYPE         Fatalities Injuries Total
##   <fct>               <dbl>    <dbl> <dbl>
## 1 TORNADO             5.63     91.3  97.0 
## 2 EXCESSIVE HEAT      1.90      6.52  8.43
## 3 TSTM WIND           0.504     6.96  7.46
## 4 FLOOD               0.47      6.79  7.26
## 5 LIGHTNING           0.816     5.23  6.05
```

#Top weather types of events with the greatest economic consequences.

Event types grouped by:
- Property damadge;
- Crop damadge;
- Property damadge + Crop damadge = Total Economic Damadge.


```r
econplot <- data2 %>% subset(select = c(EVTYPE, PROPDMGB, CROPDMGB, 
    DMGEXPB)) %>% gather(EconDMGType, Value, c(PROPDMGB, CROPDMGB, 
    DMGEXPB)) %>% group_by(EconDMGType, EVTYPE) %>% summarize(sum_econ_harm_B = sum(Value)) %>% 
    arrange(desc(sum_econ_harm_B)) %>% group_by(EconDMGType) %>% 
    slice(1:10)
```

```r
econplot$EconDMGType <- factor(econplot$EconDMGType, levels = c("PROPDMGB", 
    "CROPDMGB", "DMGEXPB"), labels = c("Property", "Crop", "Total"))
a <- ggplot(data = econplot, aes(x = reorder(EVTYPE, -sum_econ_harm_B, 
    sum), y = sum_econ_harm_B, fill = EconDMGType))
a + geom_bar(stat = "identity") + facet_grid(EconDMGType ~ ., 
    scales = "free") + geom_text(aes(label = round(sum_econ_harm_B, 
    digits = 1)), size = 3) + theme(axis.text.x = element_text(angle = 45, 
    hjust = 1)) + scale_fill_discrete(name = "Economic demadge type", 
    labels = c("Propertiy damadge", "Crop damadge", "Total economic damadge")) + 
    labs(title = "Total Economic Damadge (property & crop)", 
        x = "", y = "US $ Billions")
```

<img src="figs/unnamed-chunk-16-1.png" style="display: block; margin: auto;" />
#Summary. Top 5 weather events cused greatest economic harm.

```r
econplot %>% spread(EconDMGType, sum_econ_harm_B) %>% arrange(desc(Total)) %>% 
    head(5)
```

```
## # A tibble: 5 x 4
##   EVTYPE            Property  Crop Total
##   <fct>                <dbl> <dbl> <dbl>
## 1 FLOOD                145.   5.66 150. 
## 2 HURRICANE/TYPHOON     69.3  2.61  71.9
## 3 TORNADO               56.9 NA     57.4
## 4 STORM SURGE           43.3 NA     43.3
## 5 HAIL                  15.7  3.03  18.8
```
