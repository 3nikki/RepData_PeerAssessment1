# Reproducible Research: Peer Assessment 1

## Loading and preprocessing the data
## Convert date from factor to date data type


```r
library(dplyr)
```

```
## Warning: package 'dplyr' was built under R version 3.1.3
```

```
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(ggplot2)
```

```
## Warning: package 'ggplot2' was built under R version 3.1.3
```

```r
activity_dt <- read.csv("activity.csv")
activity_dt <- transform(activity_dt, date = as.Date(date, format="%Y-%m-%d"))
```

## Create a new variable called day type that identifies whether it is a weekday or a weekend

```r
activity_dt$day_type <- ifelse(weekdays(activity_dt$date) %in% c("Saturday","Sunday"),"Weekend","Weekday")
```

## Mean and median of the total number of steps taken per day
### First create a histogram of the total number of steps per day

```r
group_by_day <- group_by(activity_dt,date)
steps_per_day <- summarize(group_by_day,steps= sum(steps))
hist(steps_per_day$steps, main = "Total number of steps per day", xlab="Number of steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

```r
mean_steps <- mean(steps_per_day$steps,na.rm=TRUE)
mean_steps <- as.character(format(mean_steps, digits = 6))
median_steps <- median(steps_per_day$steps, na.rm=TRUE)
```

The mean number of steps is 10766.2.  
The median number of steps is 10765.


## Average daily actiity pattern

```r
group_by_interval <- group_by(activity_dt,interval)
avg_steps_interval <- summarize(group_by_interval,avg_steps=mean(steps, na.rm=TRUE))
plot(avg_steps_interval$interval, avg_steps_interval$avg_steps, type="l", xlab="Interval", ylab="Steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png) 

```r
max_interval <- avg_steps_interval[which.max(avg_steps_interval$avg_steps),]$interval
```

The 5 minute interval that on average contains the maximum number of steps is 835

## Imputing missing values

```r
num_missing<- nrow(activity_dt[is.na(activity_dt$steps),])
```


Total number of missing values is 2304.











