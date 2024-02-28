# BellaBeat-Case-Study 

### Contents: ###

* Background Info
  * Business Task
  * About the Data
  * Limitations
* Data Prepartion
* Data Exploration
* Data Visualization
* Recommendations

## Background Information

Bellabeat is a woman-focused company that creates high tech products centered around health. They offer high-tech products that aim to empower women by tracking their health through their activity, stress, sleep, menstrual cycle, and mindfullness habits. 
This includes a wellness tracker called Leaf, a wellness watch called Time, and a water bottle called Spring that helps to track hydration. Bellabeat also offers a subscription-based membership for guidance on nutrition, acitivty, sleep, and much more.

### Business Task ###

Bellabeat is a successful small company, but they are looking to expand their horizons to the global smart device market. 
Urška Sršen, Bellabeat's co-founder and Chief Creative Officer would like to see analysis on smart device fitness data to find new growth opportunities for the company. 
She has tasked the marketing analytics team to analyze this data to gain insights on how people are already using their smart devices and get recommendations for how these trends can influence Bellabeat's marketing strategy.
With this in mind, my case study aims to answer the following:
1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat's marketing strategy?

### About the Data ###

For this case study, I will be using the Fitbit Fitness Tracker Data, a public dataset that's available on Kaggle. 30 participants were selected from a survery distributed via Amazon Mechanical Turk between March 3rd, 2016 and May 12, 2016. They consented to have their data shared.

This dataset has many differenct tables within it, so I'll be using RStudio to clean, sort, and analyze the data.

### Limitations: ###

* Sample size: With only 30 participants, the data is not representative of all FitBit users. 
* Time Range: This data only covers April-May. These are typically warmer months and the activity from this dataset may be skewed.
* Reliability: There is no battery or sync data from this dataset, so we don't know if users had their FitBit die or fail to sync during the time the data was collected.
* Dated: This data is from 2016 which is now 8 years ago. In order to gain more accurate insights, it would be better to have data from the last couple of years 
* Consistency: There was no indication if some users used different models of FitBit watches. Did some have models that did not measure heart rate?
* Bias: There's a possible selection bias if users were paid to do this survey. If so, it would not be an accurate representation of the population as a whole, either.

It will be important to keep all of this in mind as we delve into the data to see what we can find.

## Data Preparation

**1. Install and Load packages**
```r
install.packages("tidyverse")
install.packages("lubridate")
install.packages("dplyr")
install.packages("ggplot2")
install.packages("tidyr")
```

```r
library(tidyverse)
library(lubridate)
library(dplyr)
library(ggplot2)
library(tidyr)
```
**2. Import Datasets from FitBit Fitness Tracker**

```r
Activity <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
Calories <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/hourlyCalories_merged.csv")
Intensities <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/hourlyIntensities_merged.csv")
Sleep <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
Weight <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv")
Heart_Rate <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/heartrate_seconds_merged.csv")
```

**3. Checking That Datasets Imported Correctly**

```r
head(Activity)
```
```r
A tibble: 6 × 16
          Id ActivityDate        TotalSteps TotalDistance TrackerDistance LoggedActivitiesDistance
       <dbl> <dttm>                   <dbl>         <dbl>           <dbl>                    <dbl>
1 1503960366 2016-04-12 00:00:00      13162          8.5             8.5                         0
2 1503960366 2016-04-13 00:00:00      10735          6.97            6.97                        0
3 1503960366 2016-04-14 00:00:00      10460          6.74            6.74                        0
4 1503960366 2016-04-15 00:00:00       9762          6.28            6.28                        0
5 1503960366 2016-04-16 00:00:00      12669          8.16            8.16                        0
6 1503960366 2016-04-17 00:00:00       9705          6.48            6.48                        0
```
```r
head(Calories)
```
```r
A tibble: 6 × 5
          Id ActivityHour        Calories time     date    
       <dbl> <dttm>                 <dbl> <chr>    <chr>   
1 1503960366 2016-04-12 00:00:00       81 00:00:00 04/12/16
2 1503960366 2016-04-12 01:00:00       61 01:00:00 04/12/16
3 1503960366 2016-04-12 02:00:00       59 02:00:00 04/12/16
4 1503960366 2016-04-12 03:00:00       47 03:00:00 04/12/16
5 1503960366 2016-04-12 04:00:00       48 04:00:00 04/12/16
6 1503960366 2016-04-12 05:00:00       48 05:00:00 04/12/16
```
```r
head(Intensities)
```
```r
A tibble: 6 × 6
          Id ActivityHour        TotalIntensity AverageIntensity time     date    
       <dbl> <dttm>                       <dbl>            <dbl> <chr>    <chr>   
1 1503960366 2016-04-12 00:00:00             20            0.333 00:00:00 04/12/16
2 1503960366 2016-04-12 01:00:00              8            0.133 01:00:00 04/12/16
3 1503960366 2016-04-12 02:00:00              7            0.117 02:00:00 04/12/16
4 1503960366 2016-04-12 03:00:00              0            0     03:00:00 04/12/16
5 1503960366 2016-04-12 04:00:00              0            0     04:00:00 04/12/16
6 1503960366 2016-04-12 05:00:00              0            0     05:00:00 04/12/16
```
```r
head(Sleep)
```
```r
A tibble: 6 × 6
          Id SleepDay            TotalSleepRecords TotalMinutesAsleep TotalTimeInBed date    
       <dbl> <dttm>                          <dbl>              <dbl>          <dbl> <chr>   
1 1503960366 2016-04-12 00:00:00                 1                327            346 04/12/16
2 1503960366 2016-04-13 00:00:00                 2                384            407 04/13/16
3 1503960366 2016-04-15 00:00:00                 1                412            442 04/15/16
4 1503960366 2016-04-16 00:00:00                 2                340            367 04/16/16
5 1503960366 2016-04-17 00:00:00                 1                700            712 04/17/16
6 1503960366 2016-04-19 00:00:00                 1                304            320 04/19/16
```
```r
head(Weight)
```
```r
A tibble: 6 × 8
          Id Date                  WeightKg WeightPounds   Fat   BMI IsManualReport         LogId
       <dbl> <chr>                    <dbl>        <dbl> <dbl> <dbl> <lgl>                  <dbl>
1 1503960366 5/2/2016 11:59:59 PM      52.6         116.    22  22.6 TRUE           1462233599000
2 1503960366 5/3/2016 11:59:59 PM      52.6         116.    NA  22.6 TRUE           1462319999000
3 1927972279 4/13/2016 1:08:52 AM     134.          294.    NA  47.5 FALSE          1460509732000
4 2873212765 4/21/2016 11:59:59 PM     56.7         125.    NA  21.5 TRUE           1461283199000
5 2873212765 5/12/2016 11:59:59 PM     57.3         126.    NA  21.7 TRUE           1463097599000
6 4319703577 4/17/2016 11:59:59 PM     72.4         160.    25  27.5 TRUE           1460937599000
```
```r
head(Heart_Rate)
```
```r
 A tibble: 6 × 5
          Id Time                Value time     date    
       <dbl> <dttm>              <dbl> <chr>    <chr>   
1 2022484408 2016-04-12 07:21:00    97 07:21:00 04/12/16
2 2022484408 2016-04-12 07:21:05   102 07:21:05 04/12/16
3 2022484408 2016-04-12 07:21:10   105 07:21:10 04/12/16
4 2022484408 2016-04-12 07:21:20   103 07:21:20 04/12/16
5 2022484408 2016-04-12 07:21:25   101 07:21:25 04/12/16
6 2022484408 2016-04-12 07:22:05    95 07:22:05 04/12/16
```

Looks like they all came in correctly! But, the dates look a bit weird and there's some formatting that will need to be fixed...

**4. Fixing Formatting**

Intensities

```r
Intensities$ActivityHour=as.POSIXct(Intensities$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
Intensities$time <- format(Intensities$ActivityHour, format = "%H:%M:%S")
Intensities$date <- format(Intensities$ActivityHour, format = "%m/%d/%y")
```
Calories

```r
Calories$ActivityHour=as.POSIXct(Calories$ActivityHour, format= "%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
Calories$time <- format(Calories$ActivityHour, format = "%H:%M:%S")
Calories$date <- format(Calories$ActivityHour, format= "%m/%d/%y")
```
Activity

```r
Activity$ActivityDate=as.POSIXct(Activity$ActivityDate, format ="%m/%d/%Y", tz=Sys.timezone())
Activity$date <- format(Activity$ActivityDate, format = "%m/%d/%y")
```

Sleep

```r
Sleep$SleepDay=as.POSIXct(Sleep$SleepDay, format="%m/%d/%Y", tz=Sys.timezone())
Sleep$date <- format(Sleep$SleepDay, format = "%m/%d/%y")
```

Heart Rate

```r
Heart_Rate$Time=as.POSIXct(Heart_Rate$Time, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
Heart_Rate$time <- format(Heart_Rate$Time, format = "%H:%M:%S")
Heart_Rate$date <- format(Heart_Rate$Time, format = "%m/%d/%y")
```
Before we continue I would also like to see the number of distinct id's for each table:

```r
> n_distinct(Activity$Id)
[1] 33
> n_distinct(Calories$Id)
[1] 33
> n_distinct(Intensities$Id)
[1] 33
> n_distinct(Sleep$Id)
[1] 24
> n_distinct(Weight$Id)
[1] 8
> n_distinct(Heart_Rate$Id)
[1] 14
```
Some notes on the above:

* Our weight dataset only has 8 unique id's. This probably means that not every participant logged their weight during the time the data was collected, so it will be hard to make any concrete conclusions.
* The sleep dataset also only has 24 unique id's. This may be because some participants did not wear their FitBit during sleep.
* There's only 14 id's for the heart rate, which surprised me at first. Most FitBits track heart rate, but since this is from 2016, it's very possible that some particpants were using an older FitBit model that did not track heart rate.
* The dataset from Kaggle states that there are 30 particpants, but we have 33 unique id's.
  * After checking the description of the dataset again, I learned that each ID represents a single export session, which means some participants exported their session multiple times

 WIth all this in mind, let's continue to explore the data now that it's been cleaned and ready to go.

## Data Exploration

**1. Dataset Summaries**

Activity

```r
Activity %>% 
  select(TotalSteps,
         TotalDistance,
         SedentaryMinutes, Calories) %>% 
  summary()
```
```r
   TotalSteps    TotalDistance    SedentaryMinutes    Calories   
 Min.   :    0   Min.   : 0.000   Min.   :   0.0   Min.   :   0  
 1st Qu.: 3790   1st Qu.: 2.620   1st Qu.: 729.8   1st Qu.:1828  
 Median : 7406   Median : 5.245   Median :1057.5   Median :2134  
 Mean   : 7638   Mean   : 5.490   Mean   : 991.2   Mean   :2304  
 3rd Qu.:10727   3rd Qu.: 7.713   3rd Qu.:1229.5   3rd Qu.:2793  
 Max.   :36019   Max.   :28.030   Max.   :1440.0   Max.   :4900  
```
Insights:

* The mean step count is 7,638. The CDC recommends that most adults aim for 10,000 steps a day. We could use this info to help Bellabeat motivate people to take more steps
* Something else that stands out is that all of the categories in the table above have minimum's of 0. This could indicate that there were some days that participants did not wear their FitBit at all.

Active Mins. per Category

```r
Activity %>% 
  select(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes) %>% 
  summary()
```
```r
VeryActiveMinutes FairlyActiveMinutes LightlyActiveMinutes
 Min.   :  0.00    Min.   :  0.00      Min.   :  0.0       
 1st Qu.:  0.00    1st Qu.:  0.00      1st Qu.:127.0       
 Median :  4.00    Median :  6.00      Median :199.0       
 Mean   : 21.16    Mean   : 13.56      Mean   :192.8       
 3rd Qu.: 32.00    3rd Qu.: 19.00      3rd Qu.:264.0       
 Max.   :210.00    Max.   :143.00      Max.   :518.0
```
Insights:

* Once again we have a minimum of zero across all categories, leading to confirm my suspicion above
* The mean of very active minutes is quite low at around 21 minutes
* The mean of fairly active minutes is somehow even lower at around 14 minutes
* The mean of lightly active minutes is the highest at around 193 minutes - that's 16 hours! While a lot of this time represents sleep hours, that's still a long time to be sedentary throughout the day

Calories

```r
Calories %>% 
  select(Calories) %>% 
  summary()
```
```r
Min.   : 42.00  
 1st Qu.: 63.00  
 Median : 83.00  
 Mean   : 97.39  
 3rd Qu.:108.00  
 Max.   :948.00
```
Insights:

* This is not 97 cals/day, rather 97 cals/hour. If we add that up to one day, it's around 2328 calories on average.
* The max cal/hour is 948. Can someone burn that many calories in one hour? This could be an outliet or a fluke in the data

Sleep

```r
Sleep %>% 
  select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed) %>% 
  summary()
```
```r
TotalSleepRecords TotalMinutesAsleep TotalTimeInBed 
 Min.   :1.000     Min.   : 58.0      Min.   : 61.0  
 1st Qu.:1.000     1st Qu.:361.0      1st Qu.:403.0  
 Median :1.000     Median :433.0      Median :463.0  
 Mean   :1.119     Mean   :419.5      Mean   :458.6  
 3rd Qu.:1.000     3rd Qu.:490.0      3rd Qu.:526.0  
 Max.   :3.000     Max.   :796.0      Max.   :961.0 
```
Insights:

* Mean minutes asleep is 419.5 or 6.99 hours
* Mean of total time in bed is 7.64 hours. That gives us a difference of .65, which means the group in this set spent an average of 35 mins awake in bed

Weight

```r
Weight %>% 
  select(WeightKg, BMI) %>% 
  summary()
```
```r
WeightKg           BMI       
 Min.   : 52.60   Min.   :21.45  
 1st Qu.: 61.40   1st Qu.:23.96  
 Median : 62.50   Median :24.39  
 Mean   : 72.04   Mean   :25.19  
 3rd Qu.: 85.05   3rd Qu.:25.56  
 Max.   :133.50   Max.   :47.54
```
Insights:

* The average weight of the 8 participants in this set is 72 kg or 158 lbs
* Highest weight is 133.5 or around 291 lbs, lowest is 52.6 or around 116 lbs. That's a good variation, but it would be great if there were more participants that also had varying weights

Heart Rate

```r
Heart_Rate %>% 
  select(Value, time, date) %>% 
  summary()
```
```r
Value            time               date          
 Min.   : 36.00   Length:2483658     Length:2483658    
 1st Qu.: 63.00   Class :character   Class :character  
 Median : 73.00   Mode  :character   Mode  :character  
 Mean   : 77.33                                        
 3rd Qu.: 88.00                                        
 Max.   :203.00           
```
Insights:

* The mean heart rate is 77 BPM, which falls in line with the average heart rate of 60-100 BPM
* The max is a staggering 203 BPM, which seems inaccurate unless this person was doing extremely vigorous activity

**2. Merging Datasets**

I'm interested in observing how sleep may relate to activity, so I will go ahead and merge those tables below:

```r
Sleep_vs._Activity <- merge(Sleep, Activity, by=c('Id', 'date'))
head(Sleep_vs._Activity)
```
    Id     date   SleepDay TotalSleepRecords TotalMinutesAsleep TotalTimeInBed ActivityDate
1 1503960366 04/12/16 2016-04-12                 1                327            346   2016-04-12
2 1503960366 04/13/16 2016-04-13                 2                384            407   2016-04-13
3 1503960366 04/15/16 2016-04-15                 1                412            442   2016-04-15
4 1503960366 04/16/16 2016-04-16                 2                340            367   2016-04-16
5 1503960366 04/17/16 2016-04-17                 1                700            712   2016-04-17
6 1503960366 04/19/16 2016-04-19                 1                304            320   2016-04-19
  TotalSteps TotalDistance TrackerDistance LoggedActivitiesDistance VeryActiveDistance
1      13162          8.50            8.50                        0               1.88
2      10735          6.97            6.97                        0               1.57
3       9762          6.28            6.28                        0               2.14
4      12669          8.16            8.16                        0               2.71
5       9705          6.48            6.48                        0               3.19
6      15506          9.88            9.88                        0               3.53
  ModeratelyActiveDistance LightActiveDistance SedentaryActiveDistance VeryActiveMinutes
1                     0.55                6.06                       0                25
2                     0.69                4.71                       0                21
3                     1.26                2.83                       0                29
4                     0.41                5.04                       0                36
5                     0.78                2.51                       0                38
6                     1.32                5.03                       0                50
  FairlyActiveMinutes LightlyActiveMinutes SedentaryMinutes Calories
1                  13                  328              728     1985
2                  19                  217              776     1797
3                  34                  209              726     1745
4                  10                  221              773     1863
5                  20                  164              539     1728
6                  31                  264              775     2035

Avg_HR <- Heart_Rate %>% 
  dplyr::group_by(date) %>% 
  summarize(mean(Value))
```
```
```

