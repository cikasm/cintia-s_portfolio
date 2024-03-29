# Data Analysis with R: Bellabeat Case Study

### Table of Contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Data Visualization](#data-visualization)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)

## Project Overview

---

Completed as part of my Google Data Analytics Certificate, this data analysis project aimed to analyze smart device fitness data to provide insights into how non-consumers of a high-tech manufacturer of health-focused products for women use smart devices. By analyzing various aspects of usage data, the business task was to identify trends, make data-driven recommendations, and inform the company's marketing strategy.

![scatter-plot-2](https://github.com/cikasm/cintia-s_portfolio/assets/139472104/8d9dfef3-8bc0-4e4d-b7d3-f36fe0e8bc73)

### Data Sources

The primary data source for this analysis was the [FitBit Fitness Tracker Data] (https://www.kaggle.com/datasets/arashnic/fitbit), a Public Domain dataset available on Kaggle. The dataset includes information about users’ fitness activities, sleep patterns, heart rate, and other relevant metrics tracked by FitBit devices. The data was generated by respondents to a distributed survey via Amazon Mechanical Turk between 03.12.2016 and 05.12.2016, and include 18 CSV files

### Tools 
* Excel/R/RStudio - Data Processing/Cleaning
* R/RStudio - Data Aggregation
* RStudio - Data Analysis
* Tidyverse/ggplot2 - Data Visualization 
* RMarkdown/HTML - Reporting 

## Data Preparation

In the initial data preparation phase, I performed the following tasks:
1. Install and load packages
2. Data loading and inspection
3. Removing duplicates and missing values
4. Data cleaning and formating

### Exploratory Data Analysis

EDA involved exploring the smart device fitness data to answer key questions, such as:

* What is the overall usage trend?
* What is the relationship between range of daily step counts and sleep duration?  

### Data Analysis
To analyze the data, I began by gathering some summary statistics and creating initial exploratory visualizations with the ggplot2 package:

``` {r}
n_distinct(Daily_Activity_Data$Id)
[1] 0
n_distinct(Sleep_Day$Id)
[1] 0
nrow(Daily_Activity_Data)
[1] 940
nrow(Sleep_Day)
[1] 413
```
I examined the sample in the dataset and found that the 30 participants had a diverse range of daily step
counts and sleep durations. However, there was a significant amount of sedentary time in their daily activities.
I also observed differences in sleep patterns, with the majority having one sleep record and a fairly consistent
sleep duration.
``` {r}
Daily_Activity_Data %>%
select(Total_Steps,
Total_Distance,
Sedentary_Minutes) %>%
summary()
```
`Total_Steps Total_Distance Sedentary_Minutes
Min. : 0 Min. : 0.000 Min. : 0.0
1st Qu.: 3790 1st Qu.: 2.620 1st Qu.: 729.8
Median : 7406 Median : 5.245 Median :1057.5
Mean : 7638 Mean : 5.490 Mean : 991.2
3rd Qu.:10727 3rd Qu.: 7.713 3rd Qu.:1229.5
Max. :36019 Max. :28.030 Max. :1440.0`

``` {r}
Sleep_Day %>%
select(Total_Sleep_Records,
Total_Minutes_Asleep,
Total_Time_In_Bed) %>%
summary()
```
`Total_Sleep_Records Total_Minutes_Asleep Total_Time_In_Bed
Min. :1.000 Min. : 58.0 Min. : 61.0 1st Qu.:1.000 1st Qu.:361.0 1st Qu.:403.0
Median :1.000 Median :433.0 Median :463.0`

#### Merging the datasets together
After combining the datasets `Sleep_Day` and `Daily_Activity_Data`, I identified a pattern regarding the
amount of time participants stay in bed and the number of steps taken; those who sleep more also take more
steps.

``` {r}
combined_data <- merge(Sleep_Day, Daily_Activity_Data, by="User_ID")
n_distinct(combined_data$Id)
```
``` {r}
activity_sleep <- combined_data %>%
select(Total_Minutes_Asleep, Total_Time_In_Bed, Total_Steps)
head(activity_sleep)
```

### Data Visualization

After gathering summary statistics, I created some exploratory visualizations using the ggplot2package.
I noticed an inverse relationship between the number of steps taken in a day and the sedentary minutes. As
the total steps increased, the sedentary minutes tended to decrease. This suggested that individuals who
were more active in terms of steps were likely to spend less time in a sedentary state.

``` {r}
ggplot(data=Daily_Activity_Data, aes(x=Total_Steps, y=Sedentary_Minutes,
color=Total_Steps)) + geom_point()
```
![scatter-plot-1](https://github.com/cikasm/cintia-s_portfolio/assets/139472104/3a9695d3-1a83-48dc-bd95-efc35b1d9ea9)

I also indentified a generally linear relationship between minutes asleep and time in bed. As expected, more
time in bed correlated with increased sleep duration. However, some participants showed unexpected trends,
spending extended time in bed with relatively lower minutes asleep.

``` {r}
ggplot(data=Sleep_Day, aes(x=Total_Minutes_Asleep, y=Total_Time_In_Bed)) + geom_point()
```

### Key Findings

The daily activity data revealed a diverse range of step counts and distances, with a notable
amount of sedentary time in participants. Sleep activity data, on the other hand, indicated variations in
sleep records, minutes asleep, and time in bed, offering a snapshot of users’ sleep patterns. Understanding
this inverse relationship between steps and sedentary minutes is crucial for Bellabeat’s marketing strategy.

In terms of daily activity, Bellabeat’s team can customize marketing strategies for those with lower step counts
by emphasizing the Leaf wellness tracker as a starting point to track their activity and sleep. Additionally,
for those with higher step counts can benefit from messages highlighting the comprehensive analysis provided
by The Bellabeat’s app to help them better understand their habits and make healthy decisions.

### Recommendations
To enrich the analysis, I would consider additional data on user demographics, such as age, gender, and
fitness levels. This information can tailor insights to specific user groups, enhancing product personalization
and marketing efforts.
