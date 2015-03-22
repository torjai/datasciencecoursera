## Project Description
The purpose of this project is to demonstrate the ability to collect, work with, and clean a data set. 
The goal is to prepare tidy data that can be used for later analysis. 

###Collection of the raw data

The dataset includes the following raw data:
=========================================
source: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

* 'features.txt': List of all features.
* 'activity_labels.txt': Links the class labels with their activity name.
* 'train/X_train.txt': Training set.
* 'train/y_train.txt': Training labels.
* 'test/X_test.txt': Test set.
* 'test/y_test.txt': Test labels.
* 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. 
Its range is from 1 to 30. 
* 'test/subject_test.txt': Each row identifies the subject who performed the activity for each window sample. 
Its range is from 1 to 30. 

##Creating the tidy datafile

0. load necessary libraries
1. merge test and train data sets
  * output: full_mean_std data frame
  * download and read raw data
  * select required measures
  * merge data sets
  * change column names and labels if necessary
2. create tidy data frame
  * output: new_summary_table data frame
  * calculate measures for each activity and for each subject separatel
  * reshape data frame to have the proper form

for detailed description see 'README.xt'

##Description of the variables in the tiny_data.txt file
1. varibles 1: feature
  * values: features indicating the mean and standard deviation for each measurements
2. variables 2-7: activity names
  * values: average of the observations for each activity 
2. variables 8-37: subjects
  * values: average of the observations for each subject 

Once the script is executed final tidy data can be read by  
import_data<-read.table("./data/new_summary_table.txt", header=T)  
View(import_data)

for further info, please, use
* str(import_data)
* dim(import_data)