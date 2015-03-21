### Course Project - Getting and Cleaning Data
library(plyr)
library(dplyr)
library(reshape2)

## 1.1 download and unzip data
if (!file.exists("Data")) {dir.create("Data")}
fileUrl = "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
fileName = "./data/runAnalysis.zip"
download.file(url=fileUrl, destfile=fileName)
unzip(zipfile=fileName, exdir="./data")
list.files("./data")

## 1.2 read activity_labels and features
activity_labels = read.table(file="./data/UCI HAR Dataset/activity_labels.txt")
      head(activity_labels); dim(activity_labels)
features = read.table(file="./data/UCI HAR Dataset/features.txt")
      head(features); dim(features)

## 1.3 read test data
X_test = read.table(file="./data/UCI HAR Dataset/test/X_test.txt")
      names(X_test) = features$V2 # 561 columns, one for each features
      str(X_test); dim(X_test)

### select coulms with "-mean()" and "-std()"
selectedCols = which(grepl("-mean()",names(X_test),fixed=T) | grepl("-std()",names(X_test),fixed=T))
      subTest = X_test[,selectedCols]
      str(subTest)

### read test activities
y_test = read.table(file="./data/UCI HAR Dataset/test/y_test.txt")
      names(y_test) = c("activity") # 2947 rows, one for each row in X_test, values for activity_labels
      head(y_test); dim(y_test); table(y_test)

### read test subjects
subject_test = read.table(file="./data/UCI HAR Dataset/test/subject_test.txt")
      names(subject_test) = c("subject") # 2947 rows, one for each row in X_test, values for subjects
      head(subject_test); dim(subject_test);table(subject_test)

### merge test data
mergedTest = cbind(y_test,subject_test,subTest)
      names(mergedTest); table(mergedTest$activity,mergedTest$subject); dim(mergedTest)

## 1.4 read train data
X_train = read.table(file="./data/UCI HAR Dataset/train/X_train.txt")
      names(X_train) = features$V2 # 561 columns, one for each features
      str(X_train); dim(X_train) 

### select coulms with "-mean()" and "-std()"
selectedCols = which(grepl("-mean()",names(X_train),fixed=T) | grepl("-std()",names(X_train),fixed=T))
      subTrain = X_train[,selectedCols]
      str(subTrain)

### read train activities
y_train = read.table(file="./data/UCI HAR Dataset/train/y_train.txt")
      names(y_train) = c("activity") # 2947 rows, one for each row in X_train, values for activity_labels
      head(y_train); dim(y_train); table(y_train)

### read train subjects
subject_train = read.table(file="./data/UCI HAR Dataset/train/subject_train.txt")
      names(subject_train) = c("subject") # 2947 rows, one for each row in X_train, values for subjects
      head(subject_train); dim(subject_train); table(subject_train)

### merge train data
mergedTrain = cbind(y_train,subject_train,subTrain)
      names(mergedTrain); table(mergedTrain$activity,mergedTrain$subject); dim(mergedTrain)

## 1.5 merge test and train data sets
full_mean_std = rbind(mergedTest,mergedTrain)
      str(full_mean_std); dim(full_mean_std)

## 1.6 replace activity ans subject values
full_mean_std$activity = activity_labels[full_mean_std$activity,2]
full_mean_std$subject = paste("subject",full_mean_std$subject,sep="_")     
      table(full_mean_std$activity)
      table(full_mean_std$subject)
      str(full_mean_std)

## 2.1 create summary data set with the average of each variable for each activity and each subject

### calculate measures for each activity and for each subject
by_activity = full_mean_std[,-2] %>% group_by(activity) %>% summarise_each(funs(mean))
      head(by_activity); dim(by_activity)
by_subject = full_mean_std[,-1] %>% group_by(subject) %>% summarise_each(funs(mean))
      head(by_subject); dim(by_subject)

### transpose by_activity and by_subject data frames
by_activity_tr = by_activity %>% melt(id=c("activity"), measure.vars=names(by_activity[,2:67])) %>% dcast(variable ~ activity)
      head(by_activity_tr); dim(by_activity_tr)
by_subject_tr = by_subject %>% melt(id=c("subject"), measure.vars=names(by_subject[,2:67])) %>% dcast(variable ~ subject)
      head(by_subject_tr); dim(by_subject_tr)

## 2.2 merge transposed data frames
new_summary_table = merge(by_activity_tr,by_subject_tr) 
names(new_summary_table)[1] = "feature"
names(new_summary_table) = tolower(names(new_summary_table))
      str(new_summary_table)

## 2.3 write the output to new_summary_table.txt
write.table(new_summary_table,file="./data/new_summary_table.txt", row.names=F)

## 3.1 read the tidy data set
import_data<-read.table("./data/new_summary_table.txt", header=T)
View(import_data)