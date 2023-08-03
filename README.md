# CaseStudy-BellaBeat
CASE STUDY: BELLABEAT FITNESS DATA ANALYSIS 

  Author: Jacqueline Rediger

  Date: July 29, 2023

This case study deminstrates the six steps of the data analysis process:
* Ask
* Prepare
* Process
* Analyze
* Share
* Act


Introduction 

BellaBeat is a high-tech company that manufactures health-focused smart products including the BellaBeat App, Leaf, Time, Spring. BellaBeat also offers a BellaBeat membership that offers a subscription-based membership program for users giving them 24/7 access to fully personalized guidance on nutrition, activity, sleep, health
and beauty, and mindfulness based on their lifestyle and goals.
BellaBeat has set their focus on creating a tech-driven wellness company for women around the world since 2013 and opened office doors around the world by 2016.


                                                      1. ASK


BUSINESS TASK: Analyze Fitbit data to gain insight and help guide marketing strategy for Bellabeat to grow as a global player.
Key Stakeholders: 
* Urška Sršen: Bellabeat’s cofounder and Chief Creative Officer
* Sando Mur: Mathematician and Bellabeat’s cofounder; key member of the Bellabeat executive team
* Bellabeat marketing analytics team



                                                     2. PREPARE


Data Source: 30 participants FitBit Fitness Tracker Data from Mobius: [Dataset Link](https://www.kaggle.com/arashnic/fitbit)
The dataset provided 18 .CSV files. 
* This dataset is under CC0: Public Domain license meaning the creator has waive his right to the work under the copyright law.
* Thirty eligible Fitbit users consented to the submission of personal
tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring.


The data follows an ROCCC approch:

* Reliability : dataset was collected from 30 FitBit users who complied consent to use personal tracker data and generated by from a distributed survey via Amazon Mechanical Turk. 
* Originality : The data is from 30 FitBit users who agreed to a personal tracker, third party data collected using Amazon Mechanical Turk.
* Comprehensive : dataset contains multiple fields on daily activity intensity, calories used, daily steps taken, daily sleep time and weight record. The dataset is limited and most data is recorded during certain days of the week.
* Current : Data was collects from the time from of March 2016 through May 2016. The data is not current in 2023, so the way users may use their FitBits now might have changed.
* Cited : data collector and source is well documented.  (CC0: Public Domain, dataset made available through Mobius)

                                                  3. PROCESS


  Google spreadsheet and R Studio Cloud (PositCloud) will be used to process the data as the tool functionality fits the purpose.

Data cleaning
sleepDay_merged.csv and weightLogInfo_merged.csv are loaded into Google sheet for data cleaning. The fields “SleepDay” and “Date” were not correctly formatted. The following steps has been done:

The date column has been select and format to ‘Date’ using spreadsheet function
Time in the column has been removed as time is irrelevant in this analysis

Data integrity
The selected data has been loaded into R Studio for analysis. The following queries have been run to check the number of unique Id in each table

* 1. First Load Packages into R:

     library("tidyverse")
     library("ggplot2")

* 2. Load CVS files containing the data we are using for this study

    set working directory
    setwd ("~/FitBit Case Study")
    loading files
    DataMerges <- read.csv("DataMerged.csv")
    DailySteps <- read.csv("DailySteps.csv")
    SleepDay <- read.csv("SleepDay.cvs")
    WeightLogInfo <- read.csv("WeightLogInfo.csv")

* 3. Identify the number of users by using the n_distinct function

     n_distinct(DataMerged$Id)
     * [1] 33
     n_distinct(DailySteps$Id)
     * [1] 33
    n_distinct(WeightLogInfo$Id)
     * [1] 8
   n_distinct(SleepDay$Id)
     * [1] 24

### Because the limited amount of data provided from the WeightLogInfo we will not be including that within our analysis

4. Data Clean Up

       head(SleepDay)
# A tibble: 6 × 5
          Id SleepDay TotalSleepRecords TotalMinutesAsleep TotalTimeInBed
* 1 1503960366 4/12/20…                 1                327            346
* 2 1503960366 4/13/20…                 2                384            407
* 3 1503960366 4/15/20…                 1                412            442
* 4 1503960366 4/16/20…                 2                340            367
* 5 1503960366 4/17/20…                 1                700            712
* 6 1503960366 4/19/20…                 1                304            320

## Previously in Google Sheets the data included date and time (12:00:00) since that time was irrelevent I removed it to make the data easier to work with.
Next, 
  # Renaming Column from SleepDay to Date
  
      colnames(SleepDay) = "Date"
      head(SleepDay)
      head(SleepDay)


          Id Date        TotalSleepRecords TotalMinutesAsleep TotalTimeInBed
* 1 1503960366 4/12/2016                 1               327           346
* 2 1503960366 4/13/2016                 2               384           407
* 3 1503960366 4/15/2016                 1               412           442
* 4 1503960366 4/16/2016                 2               340           367
* 5 1503960366 4/17/2016                 1               700           712
* 6 1503960366 4/19/2016                 1               304           320

* DataMerged (DailyActivity)

      head(DataMerged)

  #           Id ActivityDate TotalSteps TotalDistance TrackerDistance
 * 1 1503960366    4/12/2016      13162          8.50            8.50
 * 2 1503960366    4/13/2016      10735          6.97            6.97
 * 3 1503960366    4/14/2016      10460          6.74            6.74
 * 4 1503960366    4/15/2016       9762          6.28            6.28
 * 5 1503960366    4/16/2016      12669          8.16            8.16
 * 6 1503960366    4/17/2016       9705          6.48            6.48

  #   LoggedActivitiesDistance VeryActiveDistance ModeratelyActiveDistance
* 1                        0               1.88                     0.55
* 2                        0               1.57                     0.69
* 3                        0               2.44                     0.40
* 4                        0               2.14                     1.26
* 5                        0               2.71                     0.41
* 6                        0               3.19                     0.78
  #   LightActiveDistance SedentaryActiveDistance VeryActiveMinutes
* 1                6.06                       0                25
* 2                4.71                       0                21
* 3                3.91                       0                30
* 4                2.83                       0                29
* 5                5.04                       0                36
* 6                2.51                       0                38
  #   FairlyActiveMinutes LightlyActiveMinutes SedentaryMinutes Calories
* 1                  13                  328              728     1985
* 2                  19                  217              776     1797
* 3                  11                  181             1218     1776
* 4                  34                  209              726     1745
* 5                  10                  221              773     1863
* 6                  20                  164              539     1728

  # The LoggedActivitiesDistance and SedentaryActiveDistance didn't provide any necessary so I will be removing them from further analysis.

      DataMerged <- (DataMerged[c(-6, -10)])
      colnames(DataMerged)[2] = "Date"

  # View Updated Data within DataMerged

      head(DataMerged)

    ##           Id      Date TotalSteps TotalDistance TrackerDistance
* 1 1503960366 4/12/2016      13162          8.50            8.50
* 2 1503960366 4/13/2016      10735          6.97            6.97
* 3 1503960366 4/14/2016      10460          6.74            6.74
* 4 1503960366 4/15/2016       9762          6.28            6.28
* 5 1503960366 4/16/2016      12669          8.16            8.16
* 6 1503960366 4/17/2016       9705          6.48            6.48
  #   VeryActiveDistance ModeratelyActiveDistance LightActiveDistance
* 1               1.88                     0.55                6.06
* 2               1.57                     0.69                4.71
* 3               2.44                     0.40                3.91
* 4               2.14                     1.26                2.83
* 5               2.71                     0.41                5.04
* 6               3.19                     0.78                2.51
  #   VeryActiveMinutes FairlyActiveMinutes LightlyActiveMinutes SedentaryMinutes
* 1                25                  13                  328              728
* 2                21                  19                  217              776
* 3                30                  11                  181             1218
* 4                29                  34                  209              726
* 5                36                  10                  221              773
* 6                38                  20                  164              539
  #   Calories
* 1     1985
* 2     1797
* 3     1776
* 4     1745
* 5     1863
* 6     1728

  # Last, Daily Steps

      head(DailySteps)
          Id ActivityDay StepTotal
* 1 1503960366   4/12/2016     13162
* 2 1503960366   4/13/2016     10735
* 3 1503960366   4/14/2016     10460
* 4 1503960366   4/15/2016      9762
* 5 1503960366   4/16/2016     12669
* 6 1503960366   4/17/2016      9705


                                                    4. Analyze
  # Data Merged (Daily Activites)


      summary(DataMerged$TotalSteps)
   
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
   
        0    3790    7406    7638   10727   36019 
   
     
      ![image](https://github.com/Jrediger/CaseStudy-BellaBeat/assets/130318773/d062f48a-9d8b-4a45-95e6-61b97b8b7c86)

  # SleepDay
  *  Average Amount Of Sleep


           mean_sleep <- SleepDay %>%
          group_by(Id) %>%
          summarize(mean_sleep = mean(TotalMinutesAsleep)) %>%
          select(Id, mean_sleep) %>%
          arrange(mean_sleep) %>%
          as.data.frame()
          head(mean_sleep)

  #          Id mean_sleep
* 1 2320127002    61.0000
* 2 7007744171    68.5000
* 3 4558609924   127.6000
* 4 3977333714   293.6429
* 5 1644430081   294.0000
* 6 8053475328   297.0000

                                                              5. SHARE

 Graph showing steps taken daily and sedentary minutes:
   
     ggplot(data=DataMerged, aes(x=TotalSteps, y=SedentaryMinutes)) + 
      geom_point() + 
      geom_smooth() + 
      labs(title="Total Steps vs. Sedentary Minutes",
       x = "Steps", y = "Minutes")
       

![image](https://github.com/Jrediger/CaseStudy-BellaBeat/assets/130318773/a2e6e52e-f223-413e-a0f0-987c0baa7480)

 Gragh showing how much sleep each member got
 
      options(scipen = 999)
      ggplot(mean_sleep, aes(x = Id, y = mean_sleep)) +
      geom_col(aes(reorder(Id, +mean_sleep), y = mean_sleep)) +
      labs(title = "Average Minutes of Sleep", x = "Participant Id", y = "Minutes") +
      theme(axis.text.x = element_text(angle = 90)) +
      geom_hline(yintercept = mean(mean_sleep$mean_sleep), color = "red")

  
  ![image](https://github.com/Jrediger/CaseStudy-BellaBeat/assets/130318773/30ef7cc5-fd18-4ac9-8f3b-399c887876aa)

  Graph showing active steps taken in a day compared to active minutes
  
  
  ![image](https://github.com/Jrediger/CaseStudy-BellaBeat/assets/130318773/dd407f15-df32-4475-998e-8bdacd7f0f99)

                                                          6. ACT
Conclusion
Our analysis results show, it is clear that there is a clear trend in non-active people having a negative lifestyle. The three relations we found during the analysis includes:

* Very-active minutes has a positive relation to calories burnt
* Active person has a positive relation to sleep quality
* Non-active person is more likely to have a high BMI

Recommendations to business

* Bellabeat can include function in Bellabeat app to alert user who tends to have a high number to sedentary minutes
* Bellabeat can include timely notification in Leaf/Time to motivate user to move around regularly to reduce their sedentary minutes
* Bellabeat can use the relation between high sedentary minutes and BMI to promote an active lifestyle can reduce body fat and create better health with Bellabeat products
* Bellabeat can further enhance their sleep tracking function to promote the sleep/non-active relation. Use this as an incentive to purchase Bellabeat products: create a better sleeping habit by being more active in everyday life.
