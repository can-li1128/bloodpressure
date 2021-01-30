Systolic Blood Pressure
================
Can Li
2021-01-29

### **Introduction**

> This report analyzes blood pressure data from the survey collected by
> the United States National Center for Health Statistics (NCHS). This
> center has conducted a series of health and nutrition surveys since
> the 1960’s.

> Starting in 1999, about 5,000 individuals of all ages have been
> interviewed every year and then they complete the health examination
> component of the survey. Part of this dataset is made available via
> the **NHANES** package.

### **Libraries**

``` r
library("tidyverse")
library("NHANES")
```

### **Datasets**

``` r
data(NHANES)
```

### **Summary of bp data by Age Group**

    ## # A tibble: 8 x 5
    ##   AgeDecade average    sd   min   max
    ## * <fct>       <dbl> <dbl> <int> <int>
    ## 1 " 0-9"       98.6  8.76    80   132
    ## 2 " 10-19"    107.  10.7     76   146
    ## 3 " 20-29"    113.  11.7     84   179
    ## 4 " 30-39"    115.  13.0     83   182
    ## 5 " 40-49"    118.  14.5     84   209
    ## 6 " 50-59"    124.  17.1     78   226
    ## 7 " 60-69"    127.  17.3     81   212
    ## 8 " 70+"      132.  19.4     80   193

### **Summary of bp data by Age Group and Gender**

    ## # A tibble: 16 x 6
    ## # Groups:   AgeDecade [8]
    ##    AgeDecade Gender average    sd   min   max
    ##    <fct>     <fct>    <dbl> <dbl> <int> <int>
    ##  1 " 0-9"    female   100.   9.07    83   127
    ##  2 " 0-9"    male      97.4  8.32    80   132
    ##  3 " 10-19"  female   104.   9.46    79   137
    ##  4 " 10-19"  male     110.  11.2     76   146
    ##  5 " 20-29"  female   108.  10.1     84   179
    ##  6 " 20-29"  male     118.  11.3     85   179
    ##  7 " 30-39"  female   111.  12.3     83   168
    ##  8 " 30-39"  male     119.  12.3     92   182
    ##  9 " 40-49"  female   115.  14.5     84   209
    ## 10 " 40-49"  male     121.  14.0     89   185
    ## 11 " 50-59"  female   122.  16.2     78   226
    ## 12 " 50-59"  male     126.  17.8     86   221
    ## 13 " 60-69"  female   127.  17.1     82   186
    ## 14 " 60-69"  male     127.  17.5     81   212
    ## 15 " 70+"    female   134.  19.8     80   191
    ## 16 " 70+"    male     130.  18.7     80   193

### **A Boxplot to Compare bp between Gender in Age Groups**

``` r
bp_gender %>% ggplot(aes(AgeDecade, BPSysAve, color= Gender)) +
  geom_boxplot()
```

<img src="bp_files/figure-gfm/gender_diff-1.png" style="display: block; margin: auto;" />

### **Summary of bp by race as reported in the Race1 variable**

``` r
bp_race <- NHANES %>% 
  filter(Gender == "male" & AgeDecade == " 40-49" & !is.na(BPSysAve)) %>% 
  group_by(Race1) %>% select(Race1, BPSysAve) 

bp_race %>% ggplot(aes(Race1, BPSysAve, color=Race1)) +
  geom_boxplot()
```

<img src="bp_files/figure-gfm/race-diff-1.png" style="display: block; margin: auto;" />

``` r
tab_race <- bp_race %>% 
  summarize(average = mean(BPSysAve), sd = sd(BPSysAve)) %>% arrange(average)
tab_race   
```

    ## # A tibble: 5 x 3
    ##   Race1    average    sd
    ##   <fct>      <dbl> <dbl>
    ## 1 White       120.  13.4
    ## 2 Other       120.  16.2
    ## 3 Hispanic    122.  11.1
    ## 4 Mexican     122.  13.9
    ## 5 Black       126.  17.1
