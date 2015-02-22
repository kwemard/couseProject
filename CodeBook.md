

# # We first read  data set in "~/UCI HAR Dataset" (I am on MAC OS)
# We don't need the inertial folder


#---------------------------------------------------------------------------------------
## 1- Merge the training and the test sets to create one data set (data_full)
#--------------------------------------------------------------------------------
#  We first use cbind to merge "subject_test", "X_test", "y_test" to create a data called "data_test" . 
# The same for "subject_train", "X_train", "y_train" to create a data called "data_train". The complete data (data_full)
# is obtained by using rbind to "data_test" and "data_train".


#-----------------------------------------------------------------
# 2- Extracts only the measurements on the mean and standard deviation for each measurement
#---------------------------------------------------------------
# First read the "features.txt" (the complete list of variables of each feature vector), Second, select column names that include mean() and std().
# at the end. 

# To check  the above commands  you can type 
# column_names[is.valid.mean_sd].

# Now we extract measurements on the mean and standard deviation 



# Add y and subject subject variables (the last two columns in data_full )


#----------------------------------------
# 3- Uses descriptive activity names to name the activities in the data set
#-----------------------------------------------------------------

# The activity in the data set is variable "y". 
# We will  transformed this variable in factor and changed is factor levels according
# to the informations in "activity_labels.txt" :
# 1 WALKING
# 2 WALKING_UPSTAIRS
# 3 WALKING_DOWNSTAIRS
# 4 SITTING
# 5 STANDING
# 6 LAYING



#----------------------------------------------------------------
# 4- Appropriately labels the data set with descriptive variable names.
#--------------------------------------------------------------
# We use the column_names and  is.valid.mean_sd created in point 2 above.



#----------------------------------------
# 5- From the data set in step 4, creates a second, independent tidy data set with the average of each 
# variable for each activity and each subject.
#-----------------------------------

#  data set without activity and subject.

dataSET=data_clean[,-which(names(data_clean)%in%c("activity","subject"))]

data_tidy=aggregate(dataSET,list(activity = activity,subject=subject),mean)[,]
          
# write in the file tidy.txt

write.table(data_tidy,file="tidy.txt",row.name=FALSE)

