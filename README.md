##
## We first read  data set in "~/UCI HAR Dataset" (I am on MAC OS)
## We don't need the inertial folder

##-- test data
X_test <- read.table("~/UCI HAR Dataset/test/X_test.txt", quote="\"")
y_test <- read.table("~/UCI HAR Dataset/test/y_test.txt", quote="\"")
subject_test <- read.table("~/UCI HAR Dataset/test/subject_test.txt", quote="\"")


##-- train data
X_train <- read.table("~/UCI HAR Dataset/train/X_train.txt", quote="\"")
y_train <- read.table("~/UCI HAR Dataset/train/y_train.txt", quote="\"")
subject_train <- read.table("~/UCI HAR Dataset/train/subject_train.txt", quote="\"")


##---------------------------------------------------------------------------------------
# 1- Merge the training and the test sets to create one data set (data_full)
##--------------------------------------------------------------------------------
## I first use cbind to merge "subject_test", "X_test", "y_test" to create a data called "data_test" . 
##The same for "subject_train", "X_train", "y_train" to create a data called "data_train". The complete data (data_full)
## is obtained by using rbind to "data_test" and "data_train".

data_test=cbind(X_test,y_test,subject_test)
data_train=cbind(X_train,y_train,subject_train)
data_full=rbind(data_train,data_test)

##-----------------------------------------------------------------
# 2- Extracts only the measurements on the mean and standard deviation for each measurement
##---------------------------------------------------------------
## First read the "features.txt" (the complete list of variables of each feature vector), Second, select column names that include mean() and std().
## at the end. 

features <- read.table("~/UCI HAR Dataset/features.txt", quote="\"")
## column names
column_names=as.character(features[,2])
## find column where the names include "mean()" and "std()".
## The regexpr function returns a vector of the same length as "column_names"
## whit -1 where "mean()" (or "std()", depending of the pattern) no match. Type ?regexpr for more informations.

is.valid.mean=regexpr("mean()",column_names,fixed = TRUE)!= -1
is.valid.sd=regexpr("std()",column_names,fixed = TRUE)!= -1

is.valid.mean_sd=which(is.valid.mean | is.valid.sd)

## To check  the above commands  you can type 
## column_names[is.valid.mean_sd].

## Now we extract measurements on the mean and standard deviation 

data_mean_std=data_full[,is.valid.mean_sd]

## Add y and subject subject variables (the last two columns in data_full )

y=data_full[,ncol(data_full)-1]
subject=data_full[,ncol(data_full)]

data_mean_std=cbind(data_mean_std,y,subject)

##----------------------------------------
# 3- Uses descriptive activity names to name the activities in the data set
##-----------------------------------------------------------------

## The activity in the data set is variable "y". 
## We  transforme this variable in factor and changed its factor levels according
## to the informations in "activity_labels.txt" :
## 1 WALKING
## 2 WALKING_UPSTAIRS
## 3 WALKING_DOWNSTAIRS
## 4 SITTING
## 5 STANDING
## 6 LAYING

activity=as.factor(data_mean_std$y)
levels(activity)=c("WALKING","WALKING_UPSTAIRS","WALKING_DOWNSTAIRS","SITTING","STANDING","LAYING")

data_mean_std$y=activity

##----------------------------------------------------------------
# 4- Appropriately labels the data set with descriptive variable names.
##--------------------------------------------------------------
## We use the column_names and  is.valid.mean_sd created in point 2 above.

names(data_mean_std)=c(column_names[is.valid.mean_sd],"activity","subject")

data_clean=data_mean_std

##----------------------------------------
# 5- From the data set in step 4, creates a second, independent tidy data set with the average of each 
## variable for each activity and each subject.
##-----------------------------------
##  data set without "activity" and "subject" variables

dataSET=data_clean[,-which(names(data_clean)%in%c("activity","subject"))]

data_tidy=aggregate(dataSET,list(activity = activity,subject=subject),mean)[,]
          
## write in the file tidy.txt

write.table(data_tidy,file="tidy.txt",row.name=FALSE)




