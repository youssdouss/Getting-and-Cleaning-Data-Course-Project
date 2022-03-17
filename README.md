# Getting-and-Cleaning-Data-Course-Project
project to earn certificate for course 3 " Data science"

# install package and determine  work directory
install.packages("dplyr")
library("dplyr")
getwd()

# download dataset
xtrain<-read.table('./UCI HAR Dataset/train/X_train.txt', header=FALSE)
ytrain<- read.table('./UCI HAR Dataset/train/Y_train.txt', header = FALSE)

xset <- read.table('./UCI HAR Dataset/test/X_test.txt', header = FALSE)
yset <- read.table('./UCI HAR Dataset/test/Y_test.txt', header = FALSE)

features <- read.table ('./UCI HAR Dataset/features.txt', header = FALSE)
activity <- read.table('./UCI HAR Dataset/activity_labels.txt', header = FALSE)
subtrain <- read.table('./UCI HAR Dataset/train/subject_train.txt', header = FALSE)

# rename data set
subtrain<- subtrain%>%
     rename( subjectID = V1)

subtest <- read.table('./UCI HAR Dataset/test/subject_test.txt', header = FALSE)
subtest<- subtest%>%
     rename( subjectID = V1)

features <- features[,2]
features_name <- t(features)

colnames( xtrain )<- features_name
colnames( xset) <- features_name

colnames(activity) <- c( 'ID', 'activities')

# merge and combine data
x <- rbind( xtrain, xset)
y <- rbind( ytrain, yset)

sub <- rbind( subtrain, subtest)
xysub <- cbind( y, x, sub)


df <-merge( xysub, activity, by.x = 'V1', by.y = 'ID')

# select all variable contains mean and std
colNames <- colnames(df)
df2 <- df%>%
     select(activities, subjectID, grep("\\mean| \\ std", colNames))
class(df2$activities)

str( df2)
df2$activities <- as.factor(df2$activities)

# rename data data set
colnames(df2) <- gsub("^t", "time", colnames(df2))
colnames(df2)<- gsub("^f", "frequecy", colnames(df2))
colnames(df2)<-gsub("Acc", "Accelerometer", colnames(df2))
colnames(df2)<-gsub("Gyro", "Gyroscope", colnames(df2))
colnames(df2)<-gsub("Mag", "Magnitude", colnames(df2))
colnames(df2)<-gsub("BodyBody", "Body", colnames(df2))

?aggregate
#creates tidy datset independant from dataset
df3<-aggregate(. ~subjectID + activities, df2, mean)

# create a file of tidy data
write.table(df3, file = "tidydata.txt", row.names = FALSE)

str(df3)




$ timeimeBodyAccelerometer-mean()-X                                       : num  
 $ timeimeBodyAccelerometer-mean()-Y                                       : num  .
 $ timeimeBodyAccelerometer-mean()-Z                                       : num  
 $ timeimeGravityAccelerometer-mean()-X                                   : num  
 $ timeimeGravityAccelerometer-mean()-Y                                    : num  
 $ timeimeGravityAccelerometer-mean()-Z                                     : num  
 $ timeimeBodyAccelerometerJerk-mean()-X                                  : num  
 $ timeimeBodyAccelerometerJerk-mean()-Y                                   : num  
 $ timeimeBodyAccelerometerJerk-mean()-Z                                   : num  
 $ timeimeBodyGyroscope-mean()-X                                                  : num  
 $ timeimeBodyGyroscope-mean()-Y                                                  : num  
 $ timeimeBodyGyroscope-mean()-Z                                                  : num  
 $ timeimeBodyGyroscopeJerk-mean()-X                                           : num  
 $ timeimeBodyGyroscopeJerk-mean()-Y                                            : num  
 $ timeimeBodyGyroscopeJerk-mean()-Z                                            : num  
 $ timeimeBodyAccelerometerMagnitude-mean()                            : num  
 $ timeimeGravityAccelerometerMagnitude-mean()                        : num   
 $ timeimeBodyAccelerometerJerkMagnitude-mean()                     : num  
 $ timeimeBodyGyroscopeMagnitude-mean()                                    : num  
 $ timeimeBodyGyroscopeJerkMagnitude-mean()                             : num  
 $ frequecyrequecyBodyAccelerometer-mean()-X                              : num  
 $ frequecyrequecyBodyAccelerometer-mean()-Y                              : num  
 $ frequecyrequecyBodyAccelerometer-mean()-Z                              : num  
 $ frequecyrequecyBodyAccelerometer-meanFreq()-X                      : num  
 $ frequecyrequecyBodyAccelerometer-meanFreq()-Y                      : num  
 $ frequecyrequecyBodyAccelerometer-meanFreq()-Z                      : num  
 $ frequecyrequecyBodyAccelerometerJerk-mean()-X                       : num  
 $ frequecyrequecyBodyAccelerometerJerk-mean()-Y                       : num  
 $ frequecyrequecyBodyAccelerometerJerk-mean()-Z                       : num  
 $ frequecyrequecyBodyAccelerometerJerk-meanFreq()-X               : num  
 $ frequecyrequecyBodyAccelerometerJerk-meanFreq()-Y                : num  
 $ frequecyrequecyBodyAccelerometerJerk-meanFreq()-Z                : num  
 $ frequecyrequecyBodyGyroscope-mean()-X                                       : num  
 $ frequecyrequecyBodyGyroscope-mean()-Y                                       : num  
 $ frequecyrequecyBodyGyroscope-mean()-Z                                       : num  
 $ frequecyrequecyBodyGyroscope-meanFreq()-X                               : num  
 $ frequecyrequecyBodyGyroscope-meanFreq()-Y                               : num  
 $ frequecyrequecyBodyGyroscope-meanFreq()-Z                               : num  
 $ frequecyrequecyBodyAccelerometerMagnitude-mean()                : num 
 $ frequecyrequecyBodyAccelerometerMagnitude-meanFreq()        : num  
 $ frequecyrequecyBodyAccelerometerJerkMagnitude-mean()         : num  
 $ frequecyrequecyBodyAccelerometerJerkMagnitude-meanFreq() : num  
 $ frequecyrequecyBodyGyroscopeMagnitude-mean()                        : num  
 $ frequecyrequecyBodyGyroscopeMagnitude-meanFreq()                : num  
 $ frequecyrequecyBodyGyroscopeJerkMagnitude-mean()              : num  
 $ frequecyrequecyBodyGyroscopeJerkMagnitude-meanFreq().     : num
![image](https://user-images.githubusercontent.com/97908937/158908045-fdaf857f-c7d5-4150-9a65-977a34a40506.png)
