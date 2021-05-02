# Project-UCI-HAR-Dataset
Репозиторий для итоговой работы по курсу Getting and Cleaning Data.
features <- read.csv('C:/Users/Asus/Desktop/Coursera/UCI HAR Dataset/features.txt', header = FALSE, sep = ' ')
head(features)
features <- as.character(features[,2])

Merges the training and the test sets to create one data set:

data.train.x <- read.table('C:/Users/Asus/Desktop/Coursera/UCI HAR Dataset/train/X_train.txt')
data.train.activity <- read.csv('C:/Users/Asus/Desktop/Coursera/UCI HAR Dataset/train/y_train.txt', header = FALSE, sep = ' ')
data.train.subject <- read.csv('C:/Users/Asus/Desktop/Coursera/UCI HAR Dataset/train/subject_train.txt', header = FALSE, sep = ' ')
data.train <- data.frame(data.train.subject, data.train.activity, data.train.x)
names(data.train) <- c(c('subject', 'activity'), features)
head(data.train)
data.test.x <- read.table("C:/Users/Asus/Desktop/Coursera/UCI HAR Dataset/test/X_test.txt")
data.test.activity <- read.csv("C:/Users/Asus/Desktop/Coursera/UCI HAR Dataset/test/y_test.txt", header = FALSE, sep = ' ') 
data.test.subject <- read.csv("C:/Users/Asus/Desktop/Coursera/UCI HAR Dataset/test/subject_test.txt", header = FALSE, sep = ' ')
data.test <- data.frame(data.test.subject, data.test.activity, data.test.x)
names(data.test) <- c(c('subject', 'activity'), features)
head(data.test)
data.all <- rbind(data.train, data.test)
head(data.all)


----- Extracts only the measurements on the mean and standard deviation for each measurement:

select_mean.std <- grep("mean|std", features)
data_mean.std <- data.all[,c(1,2,select_mean.std + 2)]



----- Uses descriptive activity names to name the activities in the data set:

activity_labels <- read.table("C:/Users/Asus/Desktop/Coursera/UCI HAR Dataset/activity_labels.txt", header = FALSE)
activity_labels <- as.character(activity_labels[,2])
data_mean.std$activity <- activity_labels[data_mean.std$activity]
head(data_mean.std)


----- Appropriately labels the data set with descriptive variable names:

new.names <- names(data_mean.std)
new.names <- gsub("[(][)]", "", new.names)
new.names <- gsub("^t", "TimeDomain_", new.names)
new.names <- gsub("^f", "FrequencyDomain_", new.names)
new.names <- gsub("Acc", "Accelerometer", new.names)
new.names <- gsub("Gyro", "Gyroscope", new.names)
new.names <- gsub("Mag", "Magnitude", new.names)
new.names <- gsub("-mean-", "_Mean_", new.names)
new.names <- gsub("-std-", "_StandardDeviation_", new.names)
new.names <- gsub("-", "_", new.names)
names(data_mean.std) <- new.names
head(data_mean.std)


----- From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject:

str(data_mean.std)
new.data <- aggregate(data_mean.std[,3:81], by = list(activity = data_mean.std$activity, subject = data_mean.std$subject), FUN = mean)
head(new.data)
write.table(x = new.data, file = "C:/Users/Asus/Desktop/Coursera/new_data.txt", row.names = FALSE)
