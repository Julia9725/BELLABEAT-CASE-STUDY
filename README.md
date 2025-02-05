# BELLABEAT-CASE-STUDY
# INTRODUCTION
Bellabeat is a high-tech manufacturer of health-focused products for women. It is a successful small company, but they have the potential to become a larger player in the global smart device market. Collecting data on activity, sleep, stress, and reproductive health empowers women with knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women.
In order to adequately analyze these data to answer the key business questions and make recommendations, I will follow the key steps of Data Analysis Process: Ask, Prepare, Process, Analyze, Share and Act.
# ASK

# Business Task
Analyze data from the FitBit fitness tracker to see how users interact with the FitBit app and determine trends to guide marketing strategy for the company.
# KEY STAKEHOLDERS
Primary stakeholders: Urška Sršen and Sando Mur, executive team members.
Secondary stakeholders: Bellabeat marketing analytics team.
# Questions
* What are some trends in smart device usage?
* How could these trends apply to Bellabeat customers?
* How could these trends help influence Bellabeat marketing strategy?
# PREPARE
The dataset has 18 CSV. The data also follow a ROCCC approach:
* Reliability: The data is from 30 FitBit users who consented to the submission of personal tracker data and generated by from a distributed survey via Amazon Mechanical Turk.
*	Original: The data is from 30 FitBit users who consented to the submission of personal tracker data via Amazon Mechanical Turk.
*	Comprehensive: Data minute-level output for physical activity, heart rate, and sleep monitoring. While the data tracks many factors in the user activity and sleep, but the sample size is small and most data is recorded during certain days of the week.
*	Current: Data is from March 2016 to May 2016. Data is not current so the users habit may be different now.
*	Cited: Unknown
     The dataset has limitations:
 	*	Only 30 user data is available. The central limit theorem general rule of n≥30 applies and we can use the t test for statstic reference. However, a larger sample size is preferred for the analysis.
*	Upon further investigation with n_distinct() to check for unique user Id, the set has 33 user data from daily activity, 24 from sleep and only 8 from weight. There are 3 extra users and some users did not record their data for tracking daily activity and sleep.
*	For the 8 user data for weight, 5 users manually entered their weight and 3 recorded via a connected wifi device (eg: wifi scale).
*	Most data is recorded from Tuesday to Thursday, which may not be comprehensive enough to form an accurate analysis.
# Process
The following analysis will be focused on four datasets: "dailyActivity_merged" "sleepDay_merged", "weightLogInfo_merged.csv","hourlystep_merged.csv" to identify trends in user data.The processes of data cleaning, manipulation, analysis, and visualization will be performed in R.
 1. LOADING OF PACKAGES



   ```r
   install.packages("skimr")
   install.pactages("janitor")
   install.packages("tidyr")
   install.packages("dplyr")
   ```
2. Data Importation



```r
#reading the selected files with read_csv function
daily_activity <- read.csv("dailyActivity_merged.csv")
hourly_steps <- read.csv("hourlySteps_merged.csv")
daily_sleep <- read.csv("sleepDay_merged.csv")
weight <- read.csv("weightLogInfo_merged.csv")
```
# Data Cleaning
* 1. Observe and familiarize with the data
* 2.Check for null or missing values
* 3.Perform sanity check of data
  
   view and clean up dataset

  ```r
  first the daily_activity data
   head(daily_activity)
  ```
  
| **NO**|    **Id**   |  **ActivityDate**| **TotalSteps**| **TotalDistance** |**TrackerDistance**|
|-------|-------------|------------------|---------------|-------------------|-------------------|
| 1     |1503960366   |    4/12/2016     |     13162     |         8.50      |      8.50         |
| 2     |1503960366   |    4/13/2016     |     10735     |         6.97      |       6.97        |
| 3     |1503960366   |    4/14/2016     |    10460      |       6.74        |      6.74         |
| 4     |1503960366   |    4/15/2016     |      9762     |         6.28      |        6.28       |
| 5     |1503960366   |    4/16/2016     |    12669      |        8.16       |            8.16   |
| 6     |1503960366   |    4/17/2016     |     9705      |       6.48        |           6.48    |
|**NO**|   **LoggedActivitiesDistance** |**VeryActiveDistance**|**ModeratelyActiveDistance**|
| 1    |                    0           |    1.88              |       0.55                 |
| 2    |                    0           |    1.57              |       0.69                 |
| 3    |                    0           |    2.44              |       0.40                 |
| 4    |                    0           |    2.14              |       1.26                 |
| 5    |                    0           |    2.71              |      0.41                  |
| 6    |                    0           |    3.19              |       0.75                 |
|**NO**|  **LightActiveDistance** |**SedentaryActiveDistance**|**VeryActiveMinutes**|
| 1    |            6.06          |             0             |   25                |
| 2    |            4.71          |             0             |   21                |
| 3    |             3.91         |              0            |    30               |
| 4    |            2.83          |             0             |   29                |
| 5    |            5.04          |             0             |   36                |
| 6    |            2.51          |             0             |   38                |
|**NO**|   **FairlyActiveMinutes**| **LightlyActiveMinutes**| **SedentaryMinutes**| **Calories**    |
| 1    |              13          |        328              |        728          |           1985  |
| 2    |              19          |        217              |        776          |            1797 |
| 3    |              11          |        181              |        1218         |             1776|
| 4    |              34          |        209              |         726         |             1745|
| 5    |              10          |        221              |         773         |             1863|
|6     |             20           |       164               |        539          |             1728|  

  ```r

      
```r
# then the daily_sleep data
head(daily_sleep)
```
|**NO** |    **Id** |         **SleepDay** |     **TotalSleepRecords** |**TotalMinutesAsleep**|
|-------|-----------|----------------------|---------------------------|----------------------|
|    1  | 1503960366| 4/12/2016 12:00:00 AM|                1          |     327              |
|     2 | 1503960366| 4/13/2016 12:00:00 AM|                 2         |       384            |
|     3 | 1503960366| 4/15/2016 12:00:00 AM|                 1         |       412            |
|     4 | 1503960366| 4/16/2016 12:00:00 AM|                 2         |       340            |
|     5 | 1503960366| 4/17/2016 12:00:00 AM|                 1         |       700            |
|     6 | 1503960366| 4/19/2016 12:00:00 AM|                 1         |       304            |
| **NO** |            TotalTimeInBed|
|    1   |         346              |
|    2   |         407              |
|    3   |         442              |
|    4   |         367              |
|    5   |         712              |
|    6   |         320              |

```r
# And, the hourly_steps data
head(hourly_steps)
```
| **NO**|   **Id** |    **ActivityHour**   |   **StepTotal**|
|-------|----------|-----------------------|----------------|
|     1 |1503960366|  4/12/2016 12:00:00 AM|       373      | 
|     2 |1503960366|  4/12/2016 1:00:00 AM |      160       |
|     3 |1503960366|  4/12/2016 2:00:00 AM |      151       |
|     4 |1503960366|  4/12/2016 3:00:00 AM |        0       |
|     5 |1503960366|  4/12/2016 4:00:00 AM |        0       |
|     6 |1503960366|  4/12/2016 5:00:00 AM |        0       |

```r
# Finaly, the weight data
head(weight)
```
| **NO** |   **Id**   |   **Date**           | **WeightKg**|  **WeightPounds**| **Fat** | **BMI**| **IsManualReport**|
|--------|------------|----------------------|-------------|------------------|---------|--------|-------------------|
| 1      |  1503960366| 05/02/2016 23:59…    |  52.6       |  116.            |  22     |    22.6|      TRUE         | 
| 2      |  1503960366| 05/03/2016           |   52.6      |   116.           | NA      |    22.6|       TRUE        |  
|3       |  1927972279| 4/13/2016 1:08:52 AM |   134.      |    294.          | NA      |    47.5|       FALSE       |   
| 4      |  2873212765| 4/21/2016 11:59:59 PM|   56.7      |  125.            | NA      |   21.5 |       TRUE        |   
| 5      |  2873212765| 05/12/2016 23:59     | 57.3        |   126.           | NA      |   21.7 |        TRUE       |  
| 6      |  4319703577|4/17/2016 11:59:59 PM |   72.4      |   160.           | 25      |   27.5 |       TRUE        |
                      
```r
Next we check for null or missing values in the dataset.We use the function is.null()

```r
# checking and removing or null or missing value.

daily_Activity %>% is.null()
FALSE
weight %>% is.null()
FALSE
hourly_steps %>% is.null()
FALSE
daily_sleep %>% is.null()
FALSE
```r
Duplicate records are removed in each dataset. This is achieved using the function .duplicated()

```r
# remove duplicated records

daily_Activity %>% distinct()
weight %>% distinct()
hourly_steps %>% distinct()
daily_sleeP %>% distinct()
```

Lets find the  number of participants in each data set by counting
    distinct IDs
``` r
n_distinct(daily_activity$Id)
```

    ## [1] 33

``` r
n_distinct(hourly_steps$Id)
```

    ## [1] 33

``` r
n_distinct(daily_sleep$Id)
```

    ## [1] 24

``` r
n_distinct(weight$Id)
```

    ## [1] 8

Because very few participants contributed weight data, we will
exclude it from our analysis.

# Data Transformation
   Now that the data is clean, we will then perform data manipulation/transformation.

First let’s rename the ActivityDate, ActivityHour and SleepDay columns for readability purposes.

```r
"#Lets Rename these columns for proper readabilty,

 ```r
  colnames(daily_Activity)[2]="date"
 colnames(Daily_sleep)[2]="date"
 colnames(hourly_Steps)[2]="time"  
    ```
Let’s convert these renamed columns- Date, Time to DateTime

```r
colnames(daily_Activity)[2]="datetime"
 colnames(Daily_sleep)[2]="datetime"
 colnames(hourly_Steps)[2]="datetime"
  ```
 # ANALYSIS
 
