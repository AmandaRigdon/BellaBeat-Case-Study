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

For this case study, I will be using the Fitbit Fitness Tracker Data, a public dataset that's available on Kaggle. 30 participants were selected from a distributed survery via Amazon Mechanical Turk between March 3rd, 2016 and May 12, 2016. They consented to have their data shared.

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
```
install.packages("tidyverse")
install.packages("lubridate")
install.packages("dplyr")
install.packages("ggplot2")
install.packages("tidyr")
```

```
library(tidyverse)
library(lubridate)
library(dplyr)
library(ggplot2)
library(tidyr)
```
**2. Import Datasets from FitBit Fitness Tracker**

```
Activity <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
Calories <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/hourlyCalories_merged.csv")
Intensities <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/hourlyIntensities_merged.csv")
Sleep <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
Weight <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv")
Heart_Rate <- read_csv("FitBit Data/Fitabase Data 4.12.16-5.12.16/heartrate_seconds_merged.csv")
```

**3. Checking That Datasets Imported Correctly**

```
head(Activity)
```
```
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
```
head(Calories)
```
```
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
```
head(Intensities)
```
```
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
```
head(Sleep)
```
```
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
```
head(Weight)
```
```
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
```
head(Heart_Rate)
```
```
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

```
Intensities$ActivityHour=as.POSIXct(Intensities$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
Intensities$time <- format(Intensities$ActivityHour, format = "%H:%M:%S")
Intensities$date <- format(Intensities$ActivityHour, format = "%m/%d/%y")
```
