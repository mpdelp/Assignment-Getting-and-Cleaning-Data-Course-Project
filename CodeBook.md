# Cook Book
## Introduction
The script run_analysis.R performs the 5 steps described in the course project's definition. In particular:

1) All files having the same number of columns and referring to the same entities are merged using the rbind() function.

2) Columns with the mean and standard deviation measures are taken from the whole dataset.

3) These columns are labelled as per the information in features.txt.

4) Activity names and IDs are provided from activity_labels.txt for activity data with values 1:6.

5) Activity names and IDs are updated/replaced in the dataset and other column names are amended.

6) A new dataset with all the average measurements is created for each of the 30 subjects and 6 activity types and stored in a file called averages_data.txt, as per the project requirements.

## Variables

1) x_train, y_train, x_test, y_test, subject_train and subject_test contain the data from the downloaded files.

2) x_data, y_data and subject_data merge the previous datasets to further analysis.

3) features contains the correct names for the x_data dataset, which are applied to the column names stored in mean_and_std_features, a numeric vector used to extract the desired data.

4) A similar approach is taken with activity names through the activities variable.

5) all_data merges x_data, y_data and subject_data in a big dataset.

6) Finally, averages_data contains the relevant averages which will be later stored in a .txt file. ddply() from the plyr package is used to apply colMeans() and ease the development.
