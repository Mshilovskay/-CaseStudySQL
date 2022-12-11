---
## Mariia Shilovskaia - Case study: Bellabeat
---

# Introduction

This is a case study for my Google Data Analytics certificate course. In my analysis I follow the six steps of the data analysis process: ask, prepare, process, analyze, share, and act.

## Scenario

A junior data analyst from the Data Analytics team needs to process and analyze a public data set about fitness data of smart devices at Bellabeat, a high-tech manufacturer of health-focused products for women. Bellabeat’s cofounder and Chief Creative Officer believes that analyzing smart device fitness data could help unlock new growth opportunities for the company. The team needs to focus on one of Bellabeat’s products and analyze smart device data to gain insight into how consumers are using their smart devices. The insights they discover will then help guide marketing strategy for the company. The analysis needs to be presented to the Bellabeat executive team along with high-level recommendations for Bellabeat’s marketing strategy.

## Step 1: Ask

### Business Task

* Analyze smart device fitness data to gain insight into how consumers use Bellabeat devices and look for growth opportunities.
* Focus on data from Bellabeat products and gain insight into how consumers are using their smart devices.
* Present recommendations for Bellabeat’s marketing strategy.

### Stakeholders

* Urška Sršen: Bellabeat’s co-founder and Chief Creative Officer.
* Sando Mur: Mathematician and Bellabeat’s cofounder; a key member of the Bellabeat executive team.
* Bellabeat marketing analytics team: A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy. The junior analyst joined this team six months ago and has been busy learning about Bellabeat’s mission and business goals and how they can help Bellabeat to achieve them.

### Products

* Bellabeat app: The Bellabeat app provides users with health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits. This data can help users to better understand their current habits and to make healthy decisions. The Bellabeat app connects to a line of smart wellness products of the company.
* Leaf: Bellabeat’s classic wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf tracker connects to the Bellabeat app to track activity, sleep, and stress.
* Time: This wellness watch combines the timeless look of a classic timepiece with smart technology to track user activity, sleep, and stress. The Time watch connects to the Bellabeat app to provide you with insights into your daily wellness.
* Spring: This is a water bottle that tracks daily water intake using smart technology to ensure that you are appropriately hydrated throughout the day. The Spring bottle connects to the Bellabeat app to track your hydration levels.
* Bellabeat membership: Bellabeat also offers a subscription-based membership program for users. Membership gives users 24/7 access to fully personalized guidance on nutrition, activity, sleep, health and beauty, and mindfulness based on their lifestyle and goals.

## Step 2: Prepare

This case study uses the public data set that explores Bellabeat smart device users’ daily habits. The data set contains personal fitness trackers from thirty fitbit users about physical activity, weight and sleep monitoring. This Fitabase data is located at Kaggle and was collected from 4.12.2016 to 5.12.2016. Three tables from this dataset are used for analysis: dailyActivity_mergeds.csv, sleepDay_merged.csv, and weightLogInfo_merged.csv, renamed DailyActivity.csv, SleepLog.csv, and WeightLog.csv, respectively.

This data isn’t representative, because the data is too old and Bellabeat may have new products and devices since 2016. The largest dataset contains only 33 records from users, some of the columns in tables are empty. Two other tables have less data, it seems like not all users use Fitabase devices to gather data about sleep time or weight. The data comes from a third-party source with no way to examine credibility or potential biases.   

## Step 3: Process

BigQuery SQL and Excel are used for the analysis. The CSV files, DailyActivity.csv, SleepLog.csv, and WeightLog.csv have been uploaded to BigQuery, my-project-06-22-22.Fitabase.

### Inspect the length of id for all three tables with data:

```
SELECT
    DISTINCT Id
FROM `my-project-06-22-22.Fitabase.dailyActivity` 
# 33 rows with unique Id

# Checking the length of Id
SELECT
    LENGTH(CAST(Id AS STRING))
FROM `my-project-06-22-22.Fitabase.dailyActivity`

# Checking if any Id has a length less than 10 symbols
SELECT 
    LENGTH(CAST(Id AS STRING))
FROM `my-project-06-22-22.Fitabase.dailyActivity`
WHERE LENGTH(CAST(Id AS STRING)) < 10

# Checking if any Id has a length more than 10 symbols
SELECT
    LENGTH(CAST(Id AS STRING))
FROM `my-project-06-22-22.Fitabase.dailyActivity`
WHERE LENGTH(CAST(Id AS STRING)) > 10

SELECT
    DISTINCT Id
FROM `my-project-06-22-22.Fitabase.sleepDay`
# 24 rows with unique Id

# Checking the length of Id
SELECT
    LENGTH(CAST(Id AS STRING))
FROM `my-project-06-22-22.Fitabase.sleepDay`

# Checking if any Id has a length less than 10 symbols
SELECT
    LENGTH(CAST(Id AS STRING))
FROM `my-project-06-22-22.Fitabase.sleepDay`
WHERE LENGTH(CAST(Id AS STRING)) < 10

# Checking if any Id has a length more than 10 symbols
SELECT
    LENGTH(CAST(Id AS STRING))
FROM `my-project-06-22-22.Fitabase.sleepDay`
WHERE LENGTH(CAST(Id AS STRING)) > 10

SELECT
    DISTINCT Id
FROM `my-project-06-22-22.Fitabase.weightLogInfo`
# 8 rows with unique Id

# Checking the length of Id
SELECT
    LENGTH(CAST(Id AS STRING))
FROM `my-project-06-22-22.Fitabase.weightLogInfo`

# Checking if any Id has a length less than 10 symbols
SELECT
    LENGTH(CAST(Id AS STRING))
FROM `my-project-06-22-22.Fitabase.weightLogInfo`
WHERE LENGTH(CAST(Id AS STRING)) < 10

# Checking if any Id has a length more than 10 symbols
SELECT
    LENGTH(CAST(Id AS STRING))
FROM `my-project-06-22-22.Fitabase.weightLogInfo`
WHERE LENGTH(CAST(Id AS STRING)) > 10

```

### Inspect the length column for all three tables:

```
# Checking max, min, average in columns for daileActivity table
SELECT 
    MIN(TotalSteps) AS min_total_steps,
    MIN(TotalDistance) AS min_total_distance,
    MIN(Calories) AS min_calories,
    MAX(TotalSteps) AS max_total_steps,
    MAX(TotalDistance) AS max_total_distance,
    MAX(Calories) AS max_calories,
    AVG(TotalSteps) AS avg_total_steps,
    AVG(TotalDistance) AS avg_total_distance,
    AVG(Calories) AS avg_calories
FROM `my-project-06-22-22.Fitabase.dailyActivity`

# Checking max, min, average in columns for sleepDay table 
SELECT 
    MIN(TotalMinutesAsleep) as min_total_minutes_asleep,
    MIN(TotalTimeInBed) as min_total_time_in_bed,
    MAX(TotalMinutesAsleep) AS max_total_minutes_saleep,
    MAX(TotalTimeInBed) as min_total_time_in_bed,
    AVG(TotalMinutesAsleep) AS total_minutes_asleep,
    AVG(TotalTimeInBed) AS avg_total_time_in_bed
FROM `my-project-06-22-22.Fitabase.sleepDay`

# Checking max, min, average in columns for weighLogInfo table 
SELECT 
    MIN(WeightPounds) AS min_weightpounds,
    MIN(BMI) AS min_bmi,
    MAX(WeightPounds) AS max_weightpounds,
    MAX(BMI) AS max_bmi,
    AVG(WeightPounds) AS avg_weightpounds,
    AVG(BMI) AS avg_bmi
FROM `my-project-06-22-22.Fitabase.weightLogInfo`

``` 
 
### Inspect missing data for all three tables:

```
SELECT *
FROM `my-project-06-22-22.Fitabase.dailyActivity`
WHERE Id IS NULL
 
SELECT *
FROM `my-project-06-22-22.Fitabase.sleepDay`
WHERE Id IS NULL
 
SELECT *
FROM `my-project-06-22-22.Fitabase.weightLogInfo`
WHERE Id IS NULL
```

### Identify potential errors in all three tables:

```
# Checking date records in all tables: StartDate is 2016-04-12, EndDay is 2016-05-12
SELECT
    MIN(ActivityDate) AS startDate, 
    MAX(ActivityDate) AS endDate
FROM `my-project-06-22-22.Fitabase.dailyActivity` 
 
SELECT
MIN(SleepDay) AS StartDate,
MAX(SleepDay) AS EndDate
FROM `my-project-06-22-22.Fitabase.sleepDay`
 
SELECT
MIN(Date) AS startDate,
MAX(Date) AS endDate
FROM `my-project-06-22-22.Fitabase.weightLogInfo`
```

### Searching duplicate rows in tables by checking unique Id number and the date of record:

```
SELECT
    ID, ActivityDate, COUNT(*) AS NumberOfRows
FROM `my-project-06-22-22.Fitabase.dailyActivity`
GROUP BY ID, ActivityDate
HAVING NumberOfRows > 1
 
SELECT
    Id, SleepDay, COUNT(*) AS NumberOfRows
FROM `my-project-06-22-22.Fitabase.sleepDay`
GROUP BY Id, SleepDay
HAVING NumberOfRows > 1
# 3 duplicates
 
SELECT
    Id, Date, COUNT(*) AS NumberOfRows
FROM `my-project-06-22-22.Fitabase.weightLogInfo`
GROUP BY Id, Date
HAVING NumberOfRows > 1
```

### Create a new table with data and without duplicates:

```
CREATE TABLE my-project.Fitabase.sleepDayDedup
AS
SELECT
DISTINCT *
FROM `my-project-06-22-22.Fitabase.sleepDay`
 
# Checking for duplicates in sleepDayDedup:
SELECT
    Id, SleepDay, COUNT(*) AS NumberOfRows
FROM `my-project-06-22-22.Fitabase.sleepDayDedup`
GROUP BY Id, SleepDay
HAVING NumberOfRows > 1
  
SELECT
*, 
COUNT(*) AS NumberOfRows
FROM `my-project-06-22-22.Fitabase.weightLogInfo`
GROUP BY 1, 2, 3, 4, 5, 6, 7, 8
HAVING NumberOfRows > 1
```

### Ensure consistency in all three tables:


```
# Merge all data in one table:
SELECT *
FROM `my-project-06-22-22.Fitabase.dailyActivity` AS dailyActivity
LEFT JOIN
`my-project-06-22-22.Fitabase.sleepDayDedup` AS sleepDayDedup
ON dailyActivity.Id = sleepDayDedup.Id
LEFT JOIN
`my-project-06-22-22.Fitabase.weightLogInfo` AS weightLogInfo
ON dailyActivity.Id = weightLogInfo.Id ;
```

## Step 4: Analysis

### Checking average for unique users in merge table:

```
SELECT
    dailyActivity.Id,
    AVG(dailyActivity.TotalDistance) AS total_distance,
    AVG(dailyActivity.TotalSteps) AS total_steps,
    AVG(dailyActivity.Calories) AS calories,
    AVG(dailyActivity.LoggedActivitiesDistance) AS logged_activities_distance,
    AVG(dailyActivity.VeryActiveMinutes)/60 AS very_active_hours,
    AVG(dailyActivity.FairlyActiveMinutes)/60 AS fairly_active_hours,
    AVG(dailyActivity.LightlyActiveMinutes)/60 AS lightly_active_hours,
    AVG(dailyActivity.SedentaryMinutes)/60 AS sedentary_active_hours,
    AVG(sleepDayDedup.TotalMinutesAsleep)/60 AS sleep_hours,
    AVG(weightLogInfo.WeightPounds) AS avg_weight_pounds,
    AVG(weightLogInfo.BMI) AS BMI,
FROM `my-project-06-22-22.Fitabase.dailyActivity` AS dailyActivity
LEFT JOIN `my-project-06-22-22.Fitabase.sleepDayDedup`  AS sleepDayDedup ON dailyActivity.Id = sleepDayDedup.Id
LEFT JOIN `my-project-06-22-22.Fitabase.weightLogInfo` AS weightLogInfo ON dailyActivity.Id = weightLogInfo.Id
GROUP BY dailyActivity.Id;
```

### Calculating active per day for daily activity table:

``` 
SELECT
    AVG(TotalSteps) AS AvgSteps, AVG(TotalDistance) AS AvgDistance, AVG(Calories) AS     AvgCalories,
    CASE
        WHEN DayOfWeek = 1 THEN 'Sunday'
        WHEN DayOfWeek = 2 THEN 'Monday'
        WHEN DayOfWeek = 3 THEN 'Tuesday'
        WHEN DayOfWeek = 4 THEN 'Wednesday'
        WHEN DayOfWeek = 5 THEN 'Thursday'
        WHEN DayOfWeek = 6 THEN 'Friday'
        WHEN DayOfWeek = 7 THEN 'Saturday'
        END AS PartOfWeek
FROM
 (SELECT *,
 EXTRACT(DAYOFWEEK FROM ActivityDate) AS DayOfWeek
 FROM `my-project-06-22-22.Fitabase.dailyActivity`) as temp
GROUP BY DayOfWeek
ORDER BY DayOfWeek
```

### Calculating active per day for sleepDayDedup table:
 
``` 
SELECT
    AVG(TotalMinutesAsleep) AS AvgMinutesAsleep,
    AVG(TotalMinutesAsleep / 60) AS AvgHoursAsleep,
    AVG(TotalTimeInBed - TotalMinutesAsleep) AS AvgTimeInMinutesToFallAsleep,
    CASE
       WHEN DayOfWeek = 1 THEN 'Sunday'
       WHEN DayOfWeek = 2 THEN 'Monday'
       WHEN DayOfWeek = 3 THEN 'Tuesday'
       WHEN DayOfWeek = 4 THEN 'Wednesday'
       WHEN DayOfWeek = 5 THEN 'Thursday'
       WHEN DayOfWeek = 6 THEN 'Friday'
       WHEN DayOfWeek = 7 THEN 'Saturday'
 END AS PartOfWeek
FROM
 (SELECT *,
 EXTRACT(DAYOFWEEK FROM SleepDay) AS DayOfWeek
 FROM `my-project-06-22-22.Fitabase.sleepDayDedup`) as temp
GROUP BY DayOfWeek
ORDER BY DayOfWeek
```

### Calculating active per day for weightLogInfo table:

```
SELECT
    AVG(WeightPounds) AS AvgWeighPounds,
    CASE
        WHEN DayOfWeek = 1 THEN 'Sunday'
        WHEN DayOfWeek = 2 THEN 'Monday'
        WHEN DayOfWeek = 3 THEN 'Tuesday'
        WHEN DayOfWeek = 4 THEN 'Wednesday'
        WHEN DayOfWeek = 5 THEN 'Thursday'
        WHEN DayOfWeek = 6 THEN 'Friday'
        WHEN DayOfWeek = 7 THEN 'Saturday'
 END AS PartOfWeek
FROM
 (SELECT *,
 EXTRACT(DAYOFWEEK FROM Date) AS DayOfWeek
 FROM `my-project-06-22-22.Fitabase.weightLogInfo`) as temp
GROUP BY DayOfWeek
ORDER BY DayOfWeek
``` 
 
### Cheking logged and Unlogged Users

```
SELECT
 DISTINCT Id
FROM `my-project-06-22-22.Fitabase.dailyActivity`
WHERE LoggedActivitiesDistance > 0
# 4 logged Id users
 
SELECT
 DISTINCT Id
FROM `my-project-06-22-22.Fitabase.weightLogInfo`
WHERE IsManualReport = TRUE
# 5 didn’t logged Id users
 
SELECT
 DISTINCT Id
FROM `my-project-06-22-22.Fitabase.weightLogInfo`
WHERE IsManualReport = False
# 3 logged Id users
 
SELECT
 DISTINCT Id
FROM `my-project-06-22-22.Fitabase.sleepDayDedup`
# 24 Id users with records
``` 
 
## Step 5: Share

The results are explored in Google sheets and Excel.

### Logged records for all 33 unique users

* Only 12% of users logged records for their distance activity.
* 24% of users put information about the weight.
* Majority of users (72%) measures sleeping time.

Logged users are more active, in total they had more steps (8088 vs 7441) and spent more calories (2524 vs 2249) than users who didn’t logged in. The average distance for logged users is also longer (5,8 vs 5,3).
   
### Daily active

Tuesday and Saturday are more active days for users. Sunday looks like a “lazy day”.

<img src="Average steps per day of week.png"
     alt="Average steps per day of week"
     style="float: left; margin-right: 10px;" height=275 />

<img src="Average calories per day of week.png"
     alt="Average calories per day of week"
     style="float: left; margin-right: 10px;" height=280 />
     
<img src="Average distance per day of week.png"
     alt="Average distance per day of week"
     style="float: left; margin-right: 10px;" height=252 />


### Sleep hours

Sunday is also the day when users sleep more. At the same time users sleep more after active days (Tuesday and Saturday). 

<img src="Average hours of sleep per day.png"
     alt="Average hours of sleep per day"
     style="float: left; margin-right: 10px;" height=245 />

<img src="Average time to fall asleep per day.png"
     alt="Average time to fall asleep per day"
     style="float: left; margin-right: 10px;" height=260 /> 


### Weight records

On Wednesday users had the highest record of weight during the week. Maybe it happened after pretty active Tuesday, but still it only contains information from 8 users of device. 

<img src="Average weight per day of week.png"
     alt="Average weight per day of week"
     style="float: left; margin-right: 10px;" height=250 /> 

## Step 6: Act

### Recommendations

* Simplify the process of entering information for users (activities, weight, sleep hours and etc).
* Offer benefits or special health program for users who provide more accurate information and logged in device.
* Continue to collect data for future analysis.
