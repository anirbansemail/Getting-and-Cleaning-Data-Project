# Getting-and-Cleaning-Data-Project
Coursera Getting and Cleaning Data Project

Purpose of this document is to provide a summary of the “run_analysis.R” script developed to process data made available as a part of this course. There is a separate document which provides descriptions of various variables used in the R script. It is assumed that the script and the associated data files are available in the same directory.

Data provided for this course is spread across multiple files. It is assumed that the reader is familiar with the context and content of the data. At a high-level, the data set was collected from 30 subjects divided in 2 groups – train and test - that performed 6 different activities while wearing a Samsung Galaxy S smartphone. Data from 2 sensors over 3-axis was collected and processed into ~556 variables. Of these variables, only variables and mean() and std() are of interest (~86). Objective of the script is consolidate the data from various files into 1 file, provide proper labels to variables, and calculate the average of each VARIABLE for each ACTIVITY and each SUBJECT.

The script is divided in 5 sections associated with different steps required to achieve its objective.

Step-1: Merges the training and the test sets to create one data set

Script reads the data from training group - training data (X_train), subject (subject_train), and activty (y_train) and column binds these 3 data sets to create a training data set.  It them performs similar steps with data from test group. These 2 data frames are combined into 1 data frame that has data for all variables followed by subject and activity code.

Step-2: Extracts only the measurements on the mean and standard deviation for each measurement

Scripts identifies columns of interest by preforming a pattern match on “mean()” and “std()”. This matching is done on the feature data (features.txt) and then used to subset the data frame obtained in Step-1. 

Step-3: Adds descriptive activity names to name the activities in the data set 

Script adds a column to the data set with activity descriptions which to relate activity codes added in Step-1. The activity codes have been retained for visual check. Activity descriptions are “activity_labels.txt” are used in this additional column. 

Ideally, columns should also be relabeled at this stage for easier data manipulation (i.e. Step-4 of this Project). The script takes a deviation and replaces labels of the column with labels from features data frame towards the end of Step-3. This has been highlighted with comments in the script. 

Step-4:  Appropriately labels the data set with descriptive variable names

The column names have been retained as-is from features data frame. Although long, these names indicate data processing steps post data measurement. 

Step-5: Creates a 2nd independent tidy data set with average of each VARIABLE for each ACTIVITY and each SUBJECT

Script creates a group identifier variable that is a combination of subject and activity description to calculate means for each column (or variable). The script also eliminates last 2 columns (activity code and activity description) from the output to retain relevant data. Finally, it writes the data to a text file for submission.
