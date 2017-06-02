---
title: "CodeBook.md"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, warning = FALSE)
```

# Getting and Cleaning Data Course Project
### By Cissy Guan

## Introduction

### Description of the data in data.txt

the dimension is 180 x 68.

For the rows:
Since there are 30 volunteers, and each of them perform the 6 activities, i.e. walking, walking upstairs, walking downstairs, sitting, standing, and lying. That is 30 x 6 = 180 all together.  
  
For the columns:
subject: a integer variable looping through 1 to 30, labeling the identity of volunteers that gave out the observation.
activities: factors ranging from walking, walking upstairs, walking downstairs, sitting, standing, and lying, labeling the activities the observation is recording.  
  
For the following 66 columns:
recording the measurements for the features. The meanings of the variables are indicated by the variable names, and should be decyphered as the following.  

For the 1st component:
t, says it is time domain signal that were captured at a constant rate of 50 Hz
f, says it is the frequency domain signal, got from applying FFT on time domain signal

For the 2nd component: 
Acc means it is from the acclerometer in the device
Gyro means it is from the gyroscope in the device

For the 3rd component:
Body indicates it is the body acceleration signal
Gravity indicates it is the gravity acceleration signal

For the 4th component (Optional):
Jerk indicates it is obtained by deriving the body linear acceleration and angular velocity in time

For the 5th component (Optional):
Mag indicates it is the magnitude of these three-dimensional signals were calculated using the Euclidean norm

For the 6 the component:
mean says it is the mean measurement
sd says it is the standard deviation measurement

For the 7th component (Optional):
X means it measures the X axis only
Y means it measures the Y axis only
Z means it measures the Z axis only

The measurements for the features are normalized and bounded within [-1,1].They are the mean value of the mutiple observations.

### Requirments
1. A tidy data set as described below.
2. A link to a Github repository with your script for performing the analysis.
3. A code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. 
4. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.

### run_analysis.R should do
1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement.
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names.
5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

## Getting data
1. Download the data from the specified address: <https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip>.
2. Unzip the downloaded file.
3. Read through the README, features, features_into, activity_labels files.
4. Open RStudio, and set the working directory to the unziped file.
```{r directory}
setwd("~/Documents/workspace/Project")
```

## Cleaning data
1. Read the files into R objects
```{r readin}
# the file that demonstrates which subject gives the data in test file  
file1 <- "~/Documents/workspace/Project/test/subject_test.txt"
subject_test <- read.table(file1, sep = "\n")
# dim = 2947 1

# the file that gives numbers for each feature in test file
file2 <- "~/Documents/workspace/Project/test//X_test.txt"
X_test <- read.table(file2, stringsAsFactors = FALSE)
# dim = 2947 561

# the file that labels the activities thats going on in test file
file3 <- "~/Documents/workspace/Project/test/y_test.txt"
y_test <- read.table(file3, sep = "\n")
##dim = 2947 1

# the file that demonstrates which subject gives the data in train file  
file4 <- "~/Documents/workspace/Project/train/subject_train.txt"
subject_train <- read.table(file4, sep = "\n")
# dim = 7352 1

# the file that gives numbers for each feature in train file
file5 <- "~/Documents/workspace/Project/train/X_train.txt"
X_train <- read.table(file5, stringsAsFactors = FALSE)
# dim = 7352 561

# the file that labels the activities thats going on in train file

file6 <- "~/Documents/workspace/Project/train/y_train.txt"
y_train <- read.table(file6, sep = "\n")
# dim = 7352 1
```

2. Merge the train data and test data
```{r merge}
# bind the train and test features data
X <- rbind(X_train, X_test)
# dim = 10299 561

# bind the train and test activity data
y <- rbind(y_train, y_test)
# dim = 10299 1

# bind the train and test subject data
subject <- rbind(subject_train, subject_test)
# dim = 10299 1
```

3. Extracts only the measurements on the mean and standard deviation for each measurement.
The column numbers are selected based on the information provided in the feature.txt file and the feature_info.txt file
```{r extract}
X <- X[ ,c(1:6, 41:46, 81:86, 121:126, 161:166, 201:202, 214:215, 227:228, 
           240:241, 253:254, 266:271, 345:350, 424:429, 503:504, 516:517, 
           529:530, 542:543)]
```
4. Uses descriptive activity names to name the activities in the data set. According to the code of naming objects, all the object names are descriptive and in lower cases.
FOR THIS STEP, I INTENT TO DO IT AFTER GETTING THE AVERAGES, SO THAT THE AVERAGE OF ACTIVITIES WILL NOT BE NAs.
```{r activities}
names <- c("walking", "walking_up", "walking_down", 
           "sitting", "standing", "laying")
# y <- factor(y[[1]], labels = names)
```
5. Merge relevent variables together
```{r merge1}
data <- cbind(subject, y, X)
# dim = 10299 68
```
6. Appropriately labels the data set with descriptive variable names.

Name code:

* For the 1st component:
    + t says it is time domain signal that were captured at a constant rate of 50 Hz
    + f says it is the frequency domain signal, got from applying FFT on time domain signal

* For the 2nd component: 
    + Acc means it is from the acclerometer in the device
    + Gyro means it is from the gyroscope in the device

* For the 3rd component:
    + Body indicates it is the body acceleration signal
    + Gravity indicates it is the gravity acceleration signal

* For the 4th component (Optional):
    + Jerk indicates it is obtained by deriving the body linear acceleration and angular velocity in time

* For the 5th component (Optional):
    + Mag indicates it is the magnitude of these three-dimensional signals were calculated using the Euclidean norm

* For the 6 the component:
    + mean says it is the mean measurement
    + sd says it is the standard deviation measurement

* For the 7th component (Optional):
    + X means it measures the X axis only
    + Y means it measures the Y axis only
    + Z means it measures the Z axis only
```{r names}
names(data)[1] <- "subject" 
names(data)[2] <- "activities"

names(data)[3] <- "t_Acc_Body_mean_X"
names(data)[4] <- "t_Acc_Body_mean_Y"
names(data)[5] <- "t_Acc_Body_mean_Z"
names(data)[6] <- "t_Acc_Body_std_X"
names(data)[7] <- "t_Acc_Body_std_Y"
names(data)[8] <- "t_Acc_Body_std_Z"

names(data)[9] <- "t_Acc_Gravity_mean_X"
names(data)[10] <- "t_Acc_Gravity_mean_Y"
names(data)[11] <- "t_Acc_Gravity_mean_Z"
names(data)[12] <- "t_Acc_Gravity_sd_X"
names(data)[13] <- "t_Acc_Gravity_sd_Y"
names(data)[14] <- "t_Acc_Gravity_sd_Z"

names(data)[15] <- "t_Acc_Body_Jerk_mean_X"
names(data)[16] <- "t_Acc_Body_Jerk_mean_Y"
names(data)[17] <- "t_Acc_Body_Jerk_mean_Z"
names(data)[18] <- "t_Acc_Body_Jerk_sd_X"
names(data)[19] <- "t_Acc_Body_Jerk_sd_Y"
names(data)[20] <- "t_Acc_Body_Jerk_sd_Z"

names(data)[21] <- "t_Gyro_Body_mean_X"
names(data)[22] <- "t_Gyro_Body_mean_Y"
names(data)[23] <- "t_Gyro_Body_mean_Z"
names(data)[24] <- "t_Gyro_Body_sd_X"
names(data)[25] <- "t_Gyro_Body_sd_Y"
names(data)[26] <- "t_Gyro_Body_sd_Z"

names(data)[27] <- "t_Gyro_Body_Jerk_mean_X"
names(data)[28] <- "t_Gyro_Body_Jerk_mean_Y"
names(data)[29] <- "t_Gyro_Body_Jerk_mean_Z"
names(data)[30] <- "t_Gyro_Body_Jerk_sd_X"
names(data)[31] <- "t_Gyro_Body_Jerk_sd_Y"
names(data)[32] <- "t_Gyro_Body_Jerk_sd_Z"

names(data)[33] <- "t_Acc_Body_Mag_mean"
names(data)[34] <- "t_Acc_Body_Mag_sd"

names(data)[35] <- "t_Acc_Gravity_Mag_mean"
names(data)[36] <- "t_Acc_Gravity_Mag_sd"

names(data)[37] <- "t_Acc_Body_Jerk_Mag_mean"
names(data)[38] <- "t_Acc_Body_Jerk_Mag_mean"

names(data)[39] <- "t_Gyro_Body_Mag_mean"
names(data)[40] <- "t_Gyro_Body_Mag_sd"

names(data)[41] <- "t_Gyro_Body_Jerk_Mag_mean"
names(data)[42] <- "t_Gyro_Body_Jerk_Mag_sd"

names(data)[43] <- "f_Acc_Body_mean_X"
names(data)[44] <- "f_Acc_Body_mean_Y"
names(data)[45] <- "f_Acc_Body_mean_Z"
names(data)[46] <- "f_Acc_Body_sd_X"
names(data)[47] <- "f_Acc_Body_sd_Y"
names(data)[48] <- "f_Acc_Body_sd_Z"

names(data)[49] <- "f_Acc_Body_Jerk_mean_X"
names(data)[50] <- "f_Acc_Body_Jerk_mean_Y"
names(data)[51] <- "f_Acc_Body_Jerk_mean_Z"
names(data)[52] <- "f_Acc_Body_Jerk_sd_X"
names(data)[53] <- "f_Acc_Body_Jerk_sd_Y"
names(data)[54] <- "f_Acc_Body_Jerk_sd_Z"

names(data)[55] <- "f_Gyro_Body_mean_X"
names(data)[56] <- "f_Gyro_Body_mean_Y"
names(data)[57] <- "f_Gyro_Body_mean_Z"
names(data)[58] <- "f_Gyro_Body_sd_X"
names(data)[59] <- "f_Gyro_Body_sd_Y"
names(data)[60] <- "f_Gyro_Body_sd_Z"

names(data)[61] <- "f_Acc_Body_Mag_mean"
names(data)[62] <- "f_Acc_Body_Mag_sd"

names(data)[63] <- "f_Acc_Body_Jerk_Mag_mean"
names(data)[64] <- "f_Acc_Body_Jerk_Mag_sd"

names(data)[65] <- "f_Gyro_Body_Mag_mean"
names(data)[66] <- "f_Gyro_Body_Mag_sd"

names(data)[67] <- "f_Gyro_Body_Jerk_Mag_mean"
names(data)[68] <- "f_Gyro_Body_Jerk_Mag_sd"
```
Now that the data is a tidy and clean data set.
* Each measurement and variable is in one column.
* Each different observation of that variable should be in a different row
* Each variable is descriptively named.

7. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

* Firstly, I split the whole data set by the 6 kinds of activities.
* Then, I set up 6 matrix to record the summary for each activity.
* Next, looping through the 6 activities, I further split each data frame by subjects, i.e. the volunteer who conducted the data collection process, and take average for each measurement. 
* Finally, I bind all data sets together. Give the six activities descriptive names, which I delibrately saved until after calculating the means.  

```{r summerize}
data_by_activities <- split(data, data$activities)
conclusion <- list(df1 = matrix(nrow = 30, ncol = 68),
                   df2 = matrix(nrow = 30, ncol = 68),
                   df3 = matrix(nrow = 30, ncol = 68),
                   df4 = matrix(nrow = 30, ncol = 68),
                   df5 = matrix(nrow = 30, ncol = 68),
                   df6 = matrix(nrow = 30, ncol = 68))

for(i in 1:6){
    this = split(data_by_activities[[i]], data_by_activities[[i]]$subject)
    for(j in 1:length(this)){
        conclusion[[i]][j, ] = sapply(this[[j]], mean)
    }

    conclusion[[i]] = as.data.frame(conclusion[[i]])
    
}
answer <- rbind(conclusion[[1]], conclusion[[2]], conclusion[[3]],
                conclusion[[4]], conclusion[[5]], conclusion[[6]])
answer[ ,2] = factor(answer[ ,2], labels = names)
names(answer) = names(data)
```
8. Write the data set into a .csv file
```{r write}
write.table(answer, "~/Documents/workspace/Project/data.txt")
```
9. If you want to read the data back into R
```{r back}
data <- read.table("~/Documents/workspace/Project/data.txt", header - TRUE)
```