# Getting And Cleaning Data

Repo for the Coursera Course Getting And Cleaning Data Course Project


"The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details." -- by Jorge L. Reyes-Ortiz, Davide Anguita, Alessandro Ghio, Luca Oneto.
Smartlab - Non Linear Complex Systems Laboratory

For each record it is provided:
======================================
The mean and standard deviation of:
- time signals and frequancy signals (from Fast Frountier Transformation of the time signals).
- from the accelerometer and gyroscope, two signal receivers in the smartphone on the volunteers' waist. 
- the body part and gravity part boot strapped from the signals 
- the jerk signal
- the magnititude signal
- the 3 dimentional, X, Y, Z axis, signals

The dataset includes the following files:
=========================================

- 'README.md' - a general guidance

- 'CodeBook.md' - a description of the data set

- 'solution.csv': the final data

- 'run_analysis': the r script

Notes: 
======
- Features are normalized and bounded within [-1,1].
- Each feature vector is a row on the text file.

For more information about this dataset contact: guanguhan@gmail.com

