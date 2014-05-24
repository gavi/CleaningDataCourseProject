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

Variable Name|Desciption|
|-------------|-----------|
subject|
activity|
tbodyaccmeanx|
tbodyaccmeany|
tbodyaccmeanz|
tbodyaccstdx|
tbodyaccstdy|
tbodyaccstdz|
tgravityaccmeanx|
tgravityaccmeany|
tgravityaccmeanz|
tgravityaccstdx|
tgravityaccstdy|
tgravityaccstdz|
tbodyaccjerkmeanx|
tbodyaccjerkmeany|
tbodyaccjerkmeanz|
tbodyaccjerkstdx|
tbodyaccjerkstdy|
tbodyaccjerkstdz|
tbodygyromeanx|
tbodygyromeany|
tbodygyromeanz|
tbodygyrostdx|
tbodygyrostdy|
tbodygyrostdz|
tbodygyrojerkmeanx|
tbodygyrojerkmeany|
tbodygyrojerkmeanz|
tbodygyrojerkstdx|
tbodygyrojerkstdy|
tbodygyrojerkstdz|
tbodyaccmagmean|
tbodyaccmagstd|
tgravityaccmagmean|
tgravityaccmagstd|
tbodyaccjerkmagmean|
tbodyaccjerkmagstd|
tbodygyromagmean|
tbodygyromagstd|
tbodygyrojerkmagmean|
tbodygyrojerkmagstd|
fbodyaccmeanx|
fbodyaccmeany|
fbodyaccmeanz|
fbodyaccstdx|
fbodyaccstdy|
fbodyaccstdz|
fbodyaccjerkmeanx|
fbodyaccjerkmeany|
fbodyaccjerkmeanz|
fbodyaccjerkstdx|
fbodyaccjerkstdy|
fbodyaccjerkstdz|
fbodygyromeanx|
fbodygyromeany|
fbodygyromeanz|
fbodygyrostdx|
fbodygyrostdy|
fbodygyrostdz|
fbodyaccmagmean|
fbodyaccmagstd|
fbodybodyaccjerkmagmean|
fbodybodyaccjerkmagstd|
fbodybodygyromagmean|
fbodybodygyromagstd|
fbodybodygyrojerkmagmean|
fbodybodygyrojerkmagstd|
