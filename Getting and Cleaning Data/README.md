### Course Project - Getting and Cleaning Data  

The run_analysis.R script executes the following steps:  
see the numbering of block of codes in run_analysis.R

Once the script is executed final tidy data can be read by  
import_data<-read.table("./data/new_summary_table.txt", header=T)  
View(import_data)

### 0.1 load necessary libraries

### 1.1 download and unzip raw data
* create a data folder if necessary
* raw data source: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

### 1.2 read activity_labels and features
* activity_labels source: UCI HAR Dataset/activity_labels.txt
* features source: UCI HAR Dataset/features.txt"

### 1.3 read test data
* X_test (measures) source: UCI HAR Dataset/test/X_test.txt
  * name X_test columns using features
  * select coulms with "-mean()" and "-std()"
* y_test (activity list) source: UCI HAR Dataset/test/y_test.txt
  * name y_test column "activity"
* subject_test (subject list) source: UCI HAR Dataset/test/subject_test.txt
  * name subject test column "subject" 
* merge test data sets

### 1.4 read train data
* X_train (measures) source: UCI HAR Dataset/train/X_train.txt
  * name X_train columns using features
  * select coulms with "-mean()" and "-std()"
* y_train (activity list) source: UCI HAR Dataset/train/y_train.txt
  * name y_train column "activity"
* subject_train (subject list) source: UCI HAR Dataset/train/subject_train.txt
  * name subject train column "subject" 
* merge train data sets

### 1.5 merge test and train data sets
* output: full_mean_std data frame

### 1.6 replace activity and subject values in full_mean_std data frame
* using descriptive labels

### 2.1 create summary data set with the average of each variable for each activity and each subject
* calculate measures for each activity and for each subject separately
* transpose by_activity and by_subject data frames

### 2.2 merge transposed data frames
* output: new_summary_table data frame
* change column names

### 2.3 write the output to new_summary_table.txt
destination: ./data/new_summary_table.txt

### 3.1 read the tidy data set (not part of run_analysis.R)
import_data<-read.table("./data/new_summary_table.txt", header=T)
View(import_data)