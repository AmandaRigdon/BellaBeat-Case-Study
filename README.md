# BellaBeat Case Study

![](./images/bellabeatcircle.png)

## :page_with_curl: Contents: ##

* :mag_right: [Background Information](#mag_right-background-information)
  * Business Task
  * About the Data
  * Limitations
* :wrench: [Data Prepartion](#wrench-data-preparation)
* :telescope: [Data Exploration](#telescope-data-exploration)
* :bar_chart: [Data Visualization](#bar_chart-data-visualization)
* :heavy_check_mark: [Recommendations](#heavy_check_mark-recommendations)

## :mag_right: Background Information

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

For this case study, I will be using the Fitbit Fitness Tracker Data, a public dataset that's available on Kaggle. 30 participants were selected from a survey distributed via Amazon Mechanical Turk between March 3rd, 2016 and May 12, 2016. They consented to have their data shared.

This dataset has many differenct tables within it, so I'll be using RStudio to clean, sort, and analyze the data.

### Limitations: ###

* Sample size: With only 30 participants, the data is not representative of all FitBit users. 
* Time Range: This data only covers April-May. These are typically warmer months and the activity from this dataset may be skewed.
* Reliability: There is no battery or sync data from this dataset, so we don't know if users had their FitBit die or fail to sync during the time the data was collected.
* Dated: This data is from 2016 which is now 8 years ago. In order to gain more accurate insights, it would be better to have data from the last couple of years 
* Consistency: There was no indication if some users used different models of FitBit watches. Did some have models that did not measure heart rate?
* Bias: There's a possible selection bias if users were paid to do this survey. If so, it would not be an accurate representation of the population as a whole, either.

It will be important to keep all of this in mind as we delve into the data to see what we can find.

## :wrench: Data Preparation

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

## :telescope: Data Exploration

### **1. Dataset Summaries**

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
:bulb: Insights:

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
:bulb: Insights:

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
:bulb: Insights:

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
:bulb: Insights:

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
:bulb: Insights:

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
:bulb: Insights:

* The mean heart rate is 77 BPM, which falls in line with the average heart rate of 60-100 BPM
* The max is a staggering 203 BPM, which seems inaccurate unless this person was doing extremely vigorous activity

### **2. Merging Datasets**

I'm interested in observing how sleep may relate to activity, so I will go ahead and merge those tables below:

```r
Sleep_vs._Activity <- merge(Sleep, Activity, by=c('Id', 'date'))
head(Sleep_vs._Activity)
```
```r
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
```
I'm also curious about heart rate vs. activity level, something that I personally track on my own FitBit. But to merge these tables first, I will have the find the average heart rate by day:

```r
Avg_HR <- Heart_Rate %>% 
  dplyr::group_by(date) %>% 
  summarize(mean(Value))
```
```r
head(Avg_HR)
 A tibble: 6 × 3
# Groups:   date [1]
  date             Id Avg_HR
  <chr>         <dbl>  <dbl>
1 04/12/16 2022484408   75.8
2 04/12/16 2347167796   86.1
3 04/12/16 4020332650   83.5
4 04/12/16 4558609924   76.6
5 04/12/16 5553957443   64.4
6 04/12/16 5577150313   65.7
```
This took a while to pull off, but I finally was able to get it right!

Now we can merge this with the Activity table!

```r
HR_vs._Activity <-merge(Avg_HR, Activity, by=c('date', 'Id'))
```
```r
head(HR_vs._Activity)

     date         Id   Avg_HR ActivityDate TotalSteps TotalDistance TrackerDistance LoggedActivitiesDistance
1 04/12/16 2022484408 75.80418   2016-04-12      11875          8.34            8.34                        0
2 04/12/16 2347167796 86.08233   2016-04-12      10113          6.83            6.83                        0
3 04/12/16 4020332650 83.49901   2016-04-12       8539          6.12            6.12                        0
4 04/12/16 4558609924 76.63938   2016-04-12       5135          3.39            3.39                        0
5 04/12/16 5553957443 64.36511   2016-04-12      11596          7.57            7.57                        0
6 04/12/16 5577150313 65.65607   2016-04-12       8135          6.08            6.08                        0

  VeryActiveDistance ModeratelyActiveDistance LightActiveDistance SedentaryActiveDistance VeryActiveMinutes
1               3.31                     0.77                4.26                       0                42
2               2.00                     0.62                4.20                       0                28
3               0.15                     0.24                5.68                       0                 4
4               0.00                     0.00                3.39                       0                 0
5               1.37                     0.79                5.41                       0                19
6               3.60                     0.38                2.10                       0                86

  FairlyActiveMinutes LightlyActiveMinutes SedentaryMinutes Calories
1                  14                  227             1157     2390
2                  13                  320              964     2344
3                  15                  331              712     3654
4                   0                  318             1122     1909
5                  13                  277              767     2026
6                  16                  140              728     3405
```

## :bar_chart: Data Visualization

### 1. Steps vs. Calories Burnt

```r
ggplot(data=Activity, aes(x=TotalSteps, y=Calories)) +
  geom_point(color='purple') + geom_smooth(color='darkgrey') + labs(title ="Total Steps vs. Calories")
```
![](./images/stepsvscals2.png)

From the plot above, we can see that the more steps the participants took, the more calories they burned throughout the day. This is obvious, but if Bellabeat wants to keep users motivated, they may offer affimartions for people as they stay active throughout the day.

### 2. Minutes Asleep vs. Total Time in Bed

```r
ggplot(data=Sleep, aes(x=TotalMinutesAsleep, y=TotalTimeInBed)) +
  geom_point(color='purple')+ labs(title ="Total Minutes Asleep vs. Total Time in Bed")
```
![](./images/minsasleepbed.png)

There seems to be a linear relationship with total minutes asleep vs. total time in bed. Bellabeat could use this information to remind users to wind down/go to bed.

### 3. Intensity by Hour

To plot this correctly, we will first need to summarize data from the Intensities table:

```r
Hourly_Intensities <- Intensities %>%
  group_by(time) %>%
  drop_na() %>%
  summarize(mean_total_int = mean(TotalIntensity))
```
```r
# A tibble: 6 × 2
  time     mean_total_int
  <chr>             <dbl>
1 00:00:00          2.13 
2 01:00:00          1.42 
3 02:00:00          1.04 
4 03:00:00          0.444
5 04:00:00          0.633
6 05:00:00          4.95 
```
Now we can plot our data!

```r
ggplot(data=Hourly_Intensities, aes(x=time, y=mean_total_int)) + geom_histogram(stat = "identity", fill='purple') +
  theme(axis.text.x = element_text(angle = 90)) + labs(title="Average Total Intensity vs. Time")
```
![](./images/intensitytime.png)

For this dataset, it looks like people were most active around 5-6pm. If people work out or walk during this time, Bellabeat could set reminders to motivate them. Either this, or Bellabeat could suggest users to get more activity throughout the day aside from just an hour out of the day.

### 4. Sedentary Minutes vs. Sleep

```r
ggplot(data=Sleep_vs._Activity, aes(x=TotalMinutesAsleep, y=SedentaryMinutes)) + 
  geom_point(color='purple') + geom_smooth(color='darkgrey') +
  labs(title="Minutes Asleep vs. Sedentary Minutes")
```
![](./images/minasleepsed.png)

There's a negative correlation here between minutes asleep and sedentary minutes. The more active you are, the better sleep you get.

### 5. Heart Rate vs. Activity Level

```r
ggplot(data=HR_vs._Activity, aes(x=Avg_HR, y=TotalSteps)) + 
  geom_point(color='purple') + geom_smooth(color='darkgrey') +
  labs(title = "Heart Rate vs. Steps")
```
![](./images/hrsteps.png)

This one is a bit inconclusive. The amount of steps taken per day may have an effect on a lower average heart rate overall.

```r
ggplot(data=HR_vs._Activity, aes(x=Avg_HR, y=SedentaryMinutes)) + 
  geom_point(color='purple') + geom_smooth(color='darkgrey') +
  labs(title = "Heart Rate vs. Sedentary Minutes")
```
![](./images/hrsedmins.png)

For this one, a negative correlation shows that the lower your sedentary minutes are, the lower your average heart rate is. Keeping a consistent level of activity has been shown to lower heart rate in the long run.

### :clipboard: Summary of Findings:

* Activity level is correlated with better sleep and a lower heart rate, which could be indicative of better heart health.
* Participants were mostly active between 5-6pm, and on average spent around 8 hours sedentary in one day. This leads me to believe they may have had office jobs that kept them sedentary throughout the day.
* Participants only averaged around 7,000 steps a day, which is lower than the CDC recommendation of 10,000. They also burned more calories per day if they took more steps.
* Activity level also has a correlation with better sleep quality.
* There's a linear relationship with time spent in bed and total sleep time, driving the importance of winding down and getting to bed to get a good night's sleep.

## :heavy_check_mark: Recommendations

![](./images/stretching.jpg)

Based on the findings above, we can recommend the following:

### Target Audience

* Since most of these participants were sedentary throughout the day, they likely work office jobs or jobs that require them to be at a computer most of the day. Bellabeat could focus on targeting their smart device towards women who work sedentary jobs to motivate them to get more activity throughout their day, as well as manage their stress.
* Weight data was scarce and we would need more to make a conclusion, but Bellabeat should, regardless, focus on empowering women of all sizes.

### App Implementations

* Bellabeat can work on motivating users by using positive motivation when someone is active to keep them going. If the app detects the person is engaging in an activity, it can send a notification to the watch that says "Keep it up!".
* Users can set their intended step goal and active minutes per day, and the Bellebeat app will celebrate when they hit their goals for the day.
* Achievements for certain milestones (i.e. 20,000 steps in a day, 30 minutes of vigorous activity) that can encourage users to maintain an active lifestyle.
* Reminders to go to sleep and wind down for bed. There could be a meditation feature as well as celebrations for when someone gets a full night's sleep (something the user can set on their own).
* Implementing a reminder to exercise during their reguarly scheduled time. In this analysis, it was 5-6pm.
* Heart rate tracking that shows their cardio health (which is calculated from the user profile and their resting heart rate). This could be very motivating for someone to see their cardio health improve over time as they are more active.


