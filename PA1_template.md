---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data

#### 1 - Load the data (i.e. read.csv())

```r
setwd("c:/Data Science/RepData")
activity <- read.csv("activity.csv")
```
#### 2 - Process/transform the data (if necessary) into a format suitable for your analysis

```r
numbersteps <- aggregate(steps~date,data=activity,sum,na.rm=TRUE)
```

## What is mean total number of steps taken per day?

#### 1 - Make a histogram of the total number of steps taken each day

```r
barplot(numbersteps$steps, names.arg = numbersteps$date)
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

#### 2 - Calculate and report the mean and median total number of steps taken per day

```r
mean(numbersteps$steps)
```

```
## [1] 10766.19
```

```r
median(numbersteps$steps)
```

```
## [1] 10765
```

## What is the average daily activity pattern?
#### 1 - Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)
![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

#### 2 - Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
average[which.max(average$steps),]
```

```
##     interval    steps
## 104      835 206.1698
```

## Imputing missing values
#### 1 - Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```r
missing <- is.na(activity$steps)
sum(missing)
```

```
## [1] 2304
```
#### 2 - Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

##### The NA values were replaced by mean for that 5-minute interval.

#### 3 - Create a new dataset that is equal to the original dataset but with the missing data filled in

```r
activity_fill <- activity
activity_fill$steps[which(is.na(activity_fill$steps))] <- tapply(activity_fill$steps, activity_fill$interval, mean, na.rm=T, simplify=F )
activity_fill$steps <- as.vector(activity_fill$steps, mode="numeric")
```

#### 4 - Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```
## Warning: Ignoring unknown parameters: binwidth, bins, pad
```

![](PA1_template_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

##### The mean is the same and median is slightly higher when using an imputed mean, instead of NA.

```r
numbersteps_fill <- aggregate(steps~date,data=activity_fill,sum,na.rm=FALSE)
mean(numbersteps_fill$steps)
```

```
## [1] 10766.19
```

```r
median(numbersteps_fill$steps)
```

```
## [1] 10766.19
```
## Are there differences in activity patterns between weekdays and weekends?
#### 1 - Create a new factor variable in the dataset with two levels - “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

```r
weekday.or.weekend <- function(date) {
    day <- weekdays(date)
    if (day %in% c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday"))
        return("weekday")
    else if (day %in% c("Saturday", "Sunday"))
        return("weekend")
    else
        stop("invalid date")
}
activity_fill$date <- as.Date(activity_fill$date)
activity_fill$day <- sapply(activity_fill$date, FUN=weekday.or.weekend)
```
#### 2 - Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis).
![](PA1_template_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
    
