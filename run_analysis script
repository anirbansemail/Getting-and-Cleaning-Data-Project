
# set path to working directory which contains all data files - train, test, features

# 1. Merge the training and the test sets to create one data set
# 1.1 Reads training data (X_train), subject (subject_train), and activty (y_train)
# 1.2 Column binds these 3 data sets to create a training data set
# 1.3 Reads test related data in a similar manner to create a test data set 
# 1.4 Row bind training and test data into one data set - data_wip (data work in progress)
# 1.5 Expected output is a data frame of size rows(train+test) and columns(train/test+2)

xtrain <- read.table("X_train.txt")
subtrain <- read.table("subject_train.txt")
ytrain <- read.table("y_train.txt")

# Check dimension before column bind step
dim(xtrain); dim(subtrain); dim(ytrain)
# [1] 7352  561 # [1] 7352    1 # [1] 7352    1
train_data_wip <- cbind(xtrain, subtrain, ytrain)
dim(train_data_wip)
# [1] 7352  563

# Repeat similar steps with test related data
xtest <- read.table("X_test.txt")
subtest <- read.table("subject_test.txt")
ytest <- read.table("y_test.txt")
dim(xtest); dim(ytest); dim(subtest)
#[1] 2947  561 #[1] 2947    1 #[1] 2947    1
test_data_wip <- cbind(xtest, subtest, ytest)
dim(test_data_wip)
#[1] 2947  563

# Combine train and test data_wip into one data set - data_wip
data_wip <- rbind(train_data_wip, test_data_wip)
dim(data_wip)
#[1] 10299   563
## End of Step 1

# 2. Extract only the measurements on the mean and standard deviation for each measurement
# 2.1 Subsets data_wip by first identify features of interest in "features.txt";
# 2.2 Pattern match mean(), std() in features.txt ($V2); ignore case
# 2.3 Subset features data frame with columns from 2.2 to develop columns of interest (cofi)
# 2.4 Add 2 rows with subject and activity labels to match columns of data_wip (Step 1.2)
# 2.5 Subset data_wip with columns of interest

features <- read.table("features.txt")

library(stringr)

mean_pos_cins <- str_detect(string = features$V2, ignore.case("mean()"))
mean_cols <- features[mean_pos_cins, ] #subsets features for mean()
dim(mean_cols) # [1] 53  2

# Repeat similar steps to identify columns with std()
std_pos_cins <- str_detect(string = features$V2, ignore.case("std()"))
std_cols <- features[std_pos_cins, ]
dim(std_cols) # [1] 33  2

# Combine mean_cols and std_cols for columns of interest (cofi)
cofi <- rbind(mean_cols, std_cols)
dim(cofi) # [1] 86  2 
cofi <- cofi[order(cofi[,1]), ]

# Extract subset columns from data_wip  and apply names from feat_ofi$V2
# CAUTION: factors need to be converted to char / numeric
cofi[ ,2] <- suppressWarnings(as.character(cofi[ ,2]))
sub_label <- c(V1 = 562, V2 = "subject")
act_label <- c(V1 = 563, V2 = "activity")
cofi <- rbind(cofi, sub_label, act_label)
cofi[ ,1] <- suppressWarnings(as.numeric(cofi[ ,1]))
dim(cofi) #[1] 88  2

# Subset data_wip
data_wip <- data_wip[,cofi$V1]
dim(data_wip)
# [1] 10299    88
## End of Step 2 

#3. Add descriptive activity names to name the activities in the data set 
#3.1 Add a column with activity name (description); retain activity code for visual check 
#3.2 Create a vector (act_name) from last column of data_wip (i.e. activity code)
#3.3 Replace contents of act_name using activity names from features.txt
#3.4 Ideally columns names should be relabeled at this stage (i.e. Step 4 of Project)
#3.5 Add act_name column to data_wip

act_name <- data_wip$V1.2 # impact of unlabeled columns

act_label <- read.table("activity_labels.txt")
act_label[, 2] <- as.character(act_label[, 2])

# 3.3 Replace contents act_name with activity labels
for (i2 in 1:length(act_name)) {act_name[i2] <- act_label[act_name[i2], 2]}

#3.4 Relabel columns of data_wip with cofi$V2 (names) (i.e. Step 4 of Project)
colnames(data_wip) <- cofi$V2

#3.5 
data_wip <- cbind(data_wip, act_name)
dim(data_wip)
#[1] 10299    89
## End of Step 3 (and Step 4 in 3.4)

# 4. Appropriately label the data set with descriptive variable names
#4.1 Step was COMPLETED in STEP 3.4 ABOVE (to avoid additional steps to add a row to cofi)
#4.2 Column names have been retained as-is from features data frame
#4.3 Although long, these names indicate data processing steps post data measurement
#4.4 Elimination of characters could lead loss of fidelity

#5 Create 2nd independent tidy data set with 
#  Average of each VARIABLE for each ACTIVITY and each SUBJECT
#5.1 Create a column "sub_act" which is combination of "subject" and "act_name"
#5.2 Use "sub_act" as group identifier to calculate means for each column
#5.3 Arrange output data frame by "subject" and "activity"

library(plyr)
library(dplyr)

data_wip <- mutate(data_wip, sub_act = paste(subject, act_name, sep = "_"))
dim(data_wip) #[1] 10299    90

output <- data_wip %>% group_by(sub_act) %>% summarise_each(funs(mean))
output <- arrange(output, subject, activity)
output <- output[ ,1:88] # remove activity and act_name column
dim(output)
#[1] 180  88
## End of Step 5

# Extract text file for submission
write.table(output, "output.txt", row.names=FALSE)
