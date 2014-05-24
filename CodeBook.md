# Introduction

This code book explains how the data was transformed in to a tidy data set from the source at 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

and describes the variables in the output file. You can read more about the data set here 

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

# Program Description

The program starts by reading all the relevant data files.

```{r }
features<-read.table("UCI HAR Dataset/features.txt")
activity_labels<-read.table("UCI HAR Dataset/activity_labels.txt")
names(activity_labels)<-c("activity","description")
test_data<-read.table("UCI HAR Dataset/test/X_test.txt",header=F)
train_data<-read.table("UCI HAR Dataset/train/X_train.txt",header=F)
subject_test<-read.table("UCI HAR Dataset/test/subject_test.txt")
names(subject_test)<-c("subject")
subject_train<-read.table("UCI HAR Dataset/train/subject_train.txt")
names(subject_train)<-c("subject")
test_labels<-read.table("UCI HAR Dataset/test/y_test.txt")
names(test_labels)<-c("activity")
test_labels<-data.frame(activity=join(test_labels,activity_labels)[["description"]])
train_labels<-read.table("UCI HAR Dataset/train/y_train.txt")
names(train_labels)<-c("activity")
train_labels<-data.frame(activity=join(train_labels,activity_labels)[,"description"])
```

Next it transforms the filters only the columns necessary to create the tidy data set

```{r }
relevant_features<-features[features[["V2"]] %like% c("mean\\(\\)") | features[["V2"]] %like% c("std\\(\\)"),]
test_data<-test_data[,relevant_features[["V1"]]]
train_data<-train_data[,relevant_features[["V1"]]]
```

It cleans up the column names to remove dashes and brackets

```{r }
names(test_data)<-gsub("\\)","",gsub("\\(","",tolower(gsub("-","",relevant_features[["V2"]]))))
names(train_data)<-gsub("\\)","",gsub("\\(","",tolower(gsub("-","",relevant_features[["V2"]]))))
```

Next it column binds the the test data set and training data sets with their respective subject codes and activity

```{r }
test_data<-cbind(subject_test,test_labels,test_data)
train_data<-cbind(subject_train,train_labels,train_data)
```

Finally it row binds both the data sets
```{r }
final_data<-rbind(test_data,train_data)
````

For the final data set we are using ddply where we are grouping by subject and activity. Then calculating the colMeans using an inline function

```{r }
final_tidy<-ddply(final_data,.(subject,activity),function(x){
  colMeans(x[,c(3:68)])
})

```



# Variables in the Data Set
The following variables are present in the codebook

Variable Name|Desciption| The mean value of subject/activty combination for 
|-------------|-----------| The mean value of subject/activty combination for 
subject| The ID of the subject from 1 to 30
activity| The Descriptive activity, such as walking etc
tbodyaccmeanx| The mean value of subject/activty combination for 
tbodyaccmeany| The mean value of subject/activty combination for 
tbodyaccmeanz| The mean value of subject/activty combination for 
tbodyaccstdx| The mean value of subject/activty combination for 
tbodyaccstdy| The mean value of subject/activty combination for 
tbodyaccstdz| The mean value of subject/activty combination for 
tgravityaccmeanx| The mean value of subject/activty combination for 
tgravityaccmeany| The mean value of subject/activty combination for 
tgravityaccmeanz| The mean value of subject/activty combination for 
tgravityaccstdx| The mean value of subject/activty combination for 
tgravityaccstdy| The mean value of subject/activty combination for 
tgravityaccstdz| The mean value of subject/activty combination for 
tbodyaccjerkmeanx| The mean value of subject/activty combination for 
tbodyaccjerkmeany| The mean value of subject/activty combination for 
tbodyaccjerkmeanz| The mean value of subject/activty combination for 
tbodyaccjerkstdx| The mean value of subject/activty combination for 
tbodyaccjerkstdy| The mean value of subject/activty combination for 
tbodyaccjerkstdz| The mean value of subject/activty combination for 
tbodygyromeanx| The mean value of subject/activty combination for 
tbodygyromeany| The mean value of subject/activty combination for 
tbodygyromeanz| The mean value of subject/activty combination for 
tbodygyrostdx| The mean value of subject/activty combination for 
tbodygyrostdy| The mean value of subject/activty combination for 
tbodygyrostdz| The mean value of subject/activty combination for 
tbodygyrojerkmeanx| The mean value of subject/activty combination for 
tbodygyrojerkmeany| The mean value of subject/activty combination for 
tbodygyrojerkmeanz| The mean value of subject/activty combination for 
tbodygyrojerkstdx| The mean value of subject/activty combination for 
tbodygyrojerkstdy| The mean value of subject/activty combination for 
tbodygyrojerkstdz| The mean value of subject/activty combination for 
tbodyaccmagmean| The mean value of subject/activty combination for 
tbodyaccmagstd| The mean value of subject/activty combination for 
tgravityaccmagmean| The mean value of subject/activty combination for 
tgravityaccmagstd| The mean value of subject/activty combination for 
tbodyaccjerkmagmean| The mean value of subject/activty combination for 
tbodyaccjerkmagstd| The mean value of subject/activty combination for 
tbodygyromagmean| The mean value of subject/activty combination for 
tbodygyromagstd| The mean value of subject/activty combination for 
tbodygyrojerkmagmean| The mean value of subject/activty combination for 
tbodygyrojerkmagstd| The mean value of subject/activty combination for 
fbodyaccmeanx| The mean value of subject/activty combination for 
fbodyaccmeany| The mean value of subject/activty combination for 
fbodyaccmeanz| The mean value of subject/activty combination for 
fbodyaccstdx| The mean value of subject/activty combination for 
fbodyaccstdy| The mean value of subject/activty combination for 
fbodyaccstdz| The mean value of subject/activty combination for 
fbodyaccjerkmeanx| The mean value of subject/activty combination for 
fbodyaccjerkmeany| The mean value of subject/activty combination for 
fbodyaccjerkmeanz| The mean value of subject/activty combination for 
fbodyaccjerkstdx| The mean value of subject/activty combination for 
fbodyaccjerkstdy| The mean value of subject/activty combination for 
fbodyaccjerkstdz| The mean value of subject/activty combination for 
fbodygyromeanx| The mean value of subject/activty combination for 
fbodygyromeany| The mean value of subject/activty combination for 
fbodygyromeanz| The mean value of subject/activty combination for 
fbodygyrostdx| The mean value of subject/activty combination for 
fbodygyrostdy| The mean value of subject/activty combination for 
fbodygyrostdz| The mean value of subject/activty combination for 
fbodyaccmagmean| The mean value of subject/activty combination for 
fbodyaccmagstd| The mean value of subject/activty combination for 
fbodybodyaccjerkmagmean| The mean value of subject/activty combination for 
fbodybodyaccjerkmagstd| The mean value of subject/activty combination for 
fbodybodygyromagmean| The mean value of subject/activty combination for 
fbodybodygyromagstd| The mean value of subject/activty combination for 
fbodybodygyrojerkmagmean| The mean value of subject/activty combination for 
fbodybodygyrojerkmagstd| The mean value of subject/activty combination for 
