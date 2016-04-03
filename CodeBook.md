# Cook Book
## Introduction
The script run_analysis.R performs the steps described to deliver the project requirements. In particular:

1. All files having the same number of columns and referring to the same entities are merged using the rbind() function.
2. Columns with the mean and standard deviation measures are taken from the whole dataset.
3. These columns are labelled as per the information in features.txt.
4. Activity names and IDs are provided from activity_labels.txt for activity data with values 1:6.
5. Activity names and IDs are updated/replaced in the dataset and other column names are amended.
6. A new dataset with all the average measurements is created for each of the 30 subjects and 6 activity types and stored in a file called averages_data.txt, as per the project requirements.

### Additional Notes:
The rest of the columns in the dataset contain values of averages of mean and averages of standard deviation of individual features contained in original dataset for each "Subject" and "Activity".

Original column names denoting features has been modified to match the overall naming scheme of the dataset in order to fulfill tidy data requirements.

## Variables

#### 1) x_train, y_train, x_test, y_test, subject_train and subject_test contain the data from the downloaded files:

x_train <- read.table("train/X_train.txt")

y_train <- read.table("train/y_train.txt")

subject_train <- read.table("train/subject_train.txt")

x_test <- read.table("test/X_test.txt")

y_test <- read.table("test/y_test.txt")

subject_test <- read.table("test/subject_test.txt")

#### 2) x_data, y_data and subject_data merge the previous datasets to further analysis.

x_data <- rbind(x_train, x_test)

y_data <- rbind(y_train, y_test)

subject_data <- rbind(subject_train, subject_test)

#### 3a) features contains the correct names for the x_data dataset, which are applied to the column names stored in mean_and_std_features, a numeric vector used to extract the desired data.
 
features <- read.table("features.txt")

mean_and_std_features <- grep("-(mean|std)\\(\\)", features[, 2])

x_data <- x_data[, mean_and_std_features]

names(x_data) <- features[mean_and_std_features, 2]

#### 3b) A similar approach is taken with activity names through the activities variable.

activities <- read.table("activity_labels.txt")

y_data[, 1] <- activities[y_data[, 1], 2]

names(y_data) <- "activity"

#### 4) all_data merges x_data, y_data and subject_data in a big dataset.

all_data <- cbind(x_data, y_data, subject_data)

#### 5) Finally, averages_data contains the relevant averages which will be later stored in a .txt file. ddply() from the plyr package is used to apply colMeans() and ease the development.

averages_data <- ddply(all_data, .(subject, activity), function(x) colMeans(x[, 1:66]))

write.table(averages_data, "averages_data.txt", row.name=FALSE)
