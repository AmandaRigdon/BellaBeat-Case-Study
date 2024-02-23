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

**Background Information**

Bellabeat is a woman-focused company that creates high tech products centered around health. They offer high-tech products that aim to empower women by tracking their health through their activity, stress, sleep, menstrual cycle, and mindfullness habits. 
This includes a wellness tracker called Leaf, a wellness watch called Time, and a water bottle called Spring that helps to track hydration. Bellabeat also offers a subscription-based membership for guidance on nutrition, acitivty, sleep, and much more.

**Business Task**

Bellabeat is a successful small company, but they are looking to expand their horizons to the global smart device market. 
Urška Sršen, Bellabeat's co-founder and Chief Creative Officer would like to see analysis on smart device fitness data to find new growth opportunities for the company. 
She has tasked the marketing analytics team to analyze this data to gain insights on how people are already using their smart devices and get recommendations for how these trends can influence Bellabeat's marketing strategy.
With this in mind, my case study aims to answer the following:
1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat's marketing strategy?

**About the Data**

For this case study, I will be using the Fitbit Fitness Tracker Data, a public dataset that's available on Kaggle. 30 participants were selected from a distributed survery via Amazon Mechanical Turk between March 3rd, 2016 and May 12, 2016. They consented to have their data shared.

This dataset has many differenct tables within it, so I'll be using RStudio to clean, sort, and analyze the data.

Limitations:

* Sample size: With only 30 participants, the data is not representative of all FitBit users. 
* Time Range: This data only covers April-May. These are typically warmer months and the activity from this dataset may be skewed.
* Reliability: There is no battery or sync data from this dataset, so we don't know if users had their FitBit die or fail to sync during the time the data was collected.
* Dated: This data is from 2016 which is now 8 years ago. In order to gain more accurate insights, it would be better to have data from the last couple of years 
* Consistency: There was no indication if some users used different models of FitBit watches. Did some have models that did not measure heart rate?
* Bias: There's a possible selection bias if users were paid to do this survey. If so, it would not be an accurate representation of the population as a whole, either.

It will be important to keep all of this in mind as we delve into the data to see what we can find.

Data Preparation

1. Load packages


