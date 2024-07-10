---
title: "**Practical Machine Learning Course Project**"
author: Aneesh
output:
  html_document:
    highlight: pygments
    theme: cerulean
    keep_md: true
    toc: true
---

# Background

Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively.
These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks.
One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants.
They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. More information is available from the website here:
http://web.archive.org/web/20161224072740/http:/groupware.les.inf.puc-rio.br/har
 (see the section on the Weight Lifting Exercise Dataset).

# Exploratory Data Analysis


```r
filename1 <- 'pml-training.csv'
filename2 <- 'pml-testing.csv'

if (!file.exists(filename1) && !file.exists(filename2))
{
	download.file('https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv', filename1, method='curl')
	download.file('https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv', filename2, method='curl')
}

training_full <- read.csv(filename1)
testing <- read.csv(filename2)

dim(training_full)
```

```
## [1] 19622   160
```

```r
dim(testing)
```

```
## [1]  20 160
```

```r
str(training_full)
```

```
## 'data.frame':	19622 obs. of  160 variables:
##  $ X                       : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ user_name               : chr  "carlitos" "carlitos" "carlitos" "carlitos" ...
##  $ raw_timestamp_part_1    : int  1323084231 1323084231 1323084231 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 ...
##  $ raw_timestamp_part_2    : int  788290 808298 820366 120339 196328 304277 368296 440390 484323 484434 ...
##  $ cvtd_timestamp          : chr  "05/12/2011 11:23" "05/12/2011 11:23" "05/12/2011 11:23" "05/12/2011 11:23" ...
##  $ new_window              : chr  "no" "no" "no" "no" ...
##  $ num_window              : int  11 11 11 12 12 12 12 12 12 12 ...
##  $ roll_belt               : num  1.41 1.41 1.42 1.48 1.48 1.45 1.42 1.42 1.43 1.45 ...
##  $ pitch_belt              : num  8.07 8.07 8.07 8.05 8.07 8.06 8.09 8.13 8.16 8.17 ...
##  $ yaw_belt                : num  -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 ...
##  $ total_accel_belt        : int  3 3 3 3 3 3 3 3 3 3 ...
##  $ kurtosis_roll_belt      : chr  "" "" "" "" ...
##  $ kurtosis_picth_belt     : chr  "" "" "" "" ...
##  $ kurtosis_yaw_belt       : chr  "" "" "" "" ...
##  $ skewness_roll_belt      : chr  "" "" "" "" ...
##  $ skewness_roll_belt.1    : chr  "" "" "" "" ...
##  $ skewness_yaw_belt       : chr  "" "" "" "" ...
##  $ max_roll_belt           : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ max_picth_belt          : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ max_yaw_belt            : chr  "" "" "" "" ...
##  $ min_roll_belt           : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ min_pitch_belt          : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ min_yaw_belt            : chr  "" "" "" "" ...
##  $ amplitude_roll_belt     : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ amplitude_pitch_belt    : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ amplitude_yaw_belt      : chr  "" "" "" "" ...
##  $ var_total_accel_belt    : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ avg_roll_belt           : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ stddev_roll_belt        : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ var_roll_belt           : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ avg_pitch_belt          : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ stddev_pitch_belt       : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ var_pitch_belt          : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ avg_yaw_belt            : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ stddev_yaw_belt         : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ var_yaw_belt            : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ gyros_belt_x            : num  0 0.02 0 0.02 0.02 0.02 0.02 0.02 0.02 0.03 ...
##  $ gyros_belt_y            : num  0 0 0 0 0.02 0 0 0 0 0 ...
##  $ gyros_belt_z            : num  -0.02 -0.02 -0.02 -0.03 -0.02 -0.02 -0.02 -0.02 -0.02 0 ...
##  $ accel_belt_x            : int  -21 -22 -20 -22 -21 -21 -22 -22 -20 -21 ...
##  $ accel_belt_y            : int  4 4 5 3 2 4 3 4 2 4 ...
##  $ accel_belt_z            : int  22 22 23 21 24 21 21 21 24 22 ...
##  $ magnet_belt_x           : int  -3 -7 -2 -6 -6 0 -4 -2 1 -3 ...
##  $ magnet_belt_y           : int  599 608 600 604 600 603 599 603 602 609 ...
##  $ magnet_belt_z           : int  -313 -311 -305 -310 -302 -312 -311 -313 -312 -308 ...
##  $ roll_arm                : num  -128 -128 -128 -128 -128 -128 -128 -128 -128 -128 ...
##  $ pitch_arm               : num  22.5 22.5 22.5 22.1 22.1 22 21.9 21.8 21.7 21.6 ...
##  $ yaw_arm                 : num  -161 -161 -161 -161 -161 -161 -161 -161 -161 -161 ...
##  $ total_accel_arm         : int  34 34 34 34 34 34 34 34 34 34 ...
##  $ var_accel_arm           : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ avg_roll_arm            : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ stddev_roll_arm         : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ var_roll_arm            : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ avg_pitch_arm           : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ stddev_pitch_arm        : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ var_pitch_arm           : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ avg_yaw_arm             : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ stddev_yaw_arm          : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ var_yaw_arm             : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ gyros_arm_x             : num  0 0.02 0.02 0.02 0 0.02 0 0.02 0.02 0.02 ...
##  $ gyros_arm_y             : num  0 -0.02 -0.02 -0.03 -0.03 -0.03 -0.03 -0.02 -0.03 -0.03 ...
##  $ gyros_arm_z             : num  -0.02 -0.02 -0.02 0.02 0 0 0 0 -0.02 -0.02 ...
##  $ accel_arm_x             : int  -288 -290 -289 -289 -289 -289 -289 -289 -288 -288 ...
##  $ accel_arm_y             : int  109 110 110 111 111 111 111 111 109 110 ...
##  $ accel_arm_z             : int  -123 -125 -126 -123 -123 -122 -125 -124 -122 -124 ...
##  $ magnet_arm_x            : int  -368 -369 -368 -372 -374 -369 -373 -372 -369 -376 ...
##  $ magnet_arm_y            : int  337 337 344 344 337 342 336 338 341 334 ...
##  $ magnet_arm_z            : int  516 513 513 512 506 513 509 510 518 516 ...
##  $ kurtosis_roll_arm       : chr  "" "" "" "" ...
##  $ kurtosis_picth_arm      : chr  "" "" "" "" ...
##  $ kurtosis_yaw_arm        : chr  "" "" "" "" ...
##  $ skewness_roll_arm       : chr  "" "" "" "" ...
##  $ skewness_pitch_arm      : chr  "" "" "" "" ...
##  $ skewness_yaw_arm        : chr  "" "" "" "" ...
##  $ max_roll_arm            : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ max_picth_arm           : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ max_yaw_arm             : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ min_roll_arm            : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ min_pitch_arm           : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ min_yaw_arm             : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ amplitude_roll_arm      : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ amplitude_pitch_arm     : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ amplitude_yaw_arm       : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ roll_dumbbell           : num  13.1 13.1 12.9 13.4 13.4 ...
##  $ pitch_dumbbell          : num  -70.5 -70.6 -70.3 -70.4 -70.4 ...
##  $ yaw_dumbbell            : num  -84.9 -84.7 -85.1 -84.9 -84.9 ...
##  $ kurtosis_roll_dumbbell  : chr  "" "" "" "" ...
##  $ kurtosis_picth_dumbbell : chr  "" "" "" "" ...
##  $ kurtosis_yaw_dumbbell   : chr  "" "" "" "" ...
##  $ skewness_roll_dumbbell  : chr  "" "" "" "" ...
##  $ skewness_pitch_dumbbell : chr  "" "" "" "" ...
##  $ skewness_yaw_dumbbell   : chr  "" "" "" "" ...
##  $ max_roll_dumbbell       : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ max_picth_dumbbell      : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ max_yaw_dumbbell        : chr  "" "" "" "" ...
##  $ min_roll_dumbbell       : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ min_pitch_dumbbell      : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ min_yaw_dumbbell        : chr  "" "" "" "" ...
##  $ amplitude_roll_dumbbell : num  NA NA NA NA NA NA NA NA NA NA ...
##   [list output truncated]
```

# Loading the necessary libraries


```r
library(caret)
```

```
## Loading required package: ggplot2
```

```
## Loading required package: lattice
```

```r
library(rattle)
```

```
## Loading required package: tibble
```

```
## Loading required package: bitops
```

```
## Rattle: A free graphical interface for data science with R.
## Version 5.5.1 Copyright (c) 2006-2021 Togaware Pty Ltd.
## Type 'rattle()' to shake, rattle, and roll your data.
```

# Cleaning the data

Removing columns with missing or insufficient data:

```r
trainingClean <- training_full[,colSums(is.na(training_full) | training_full == '' | training_full == ' #DIV/0!') == 0]
```

Removing unnecessary variables:

```r
trainingClean <- trainingClean[,-c(1:7)] # removing metadata

head(trainingClean, 2)
```

```
##   roll_belt pitch_belt yaw_belt total_accel_belt gyros_belt_x gyros_belt_y
## 1      1.41       8.07    -94.4                3         0.00            0
## 2      1.41       8.07    -94.4                3         0.02            0
##   gyros_belt_z accel_belt_x accel_belt_y accel_belt_z magnet_belt_x
## 1        -0.02          -21            4           22            -3
## 2        -0.02          -22            4           22            -7
##   magnet_belt_y magnet_belt_z roll_arm pitch_arm yaw_arm total_accel_arm
## 1           599          -313     -128      22.5    -161              34
## 2           608          -311     -128      22.5    -161              34
##   gyros_arm_x gyros_arm_y gyros_arm_z accel_arm_x accel_arm_y accel_arm_z
## 1        0.00        0.00       -0.02        -288         109        -123
## 2        0.02       -0.02       -0.02        -290         110        -125
##   magnet_arm_x magnet_arm_y magnet_arm_z roll_dumbbell pitch_dumbbell
## 1         -368          337          516      13.05217      -70.49400
## 2         -369          337          513      13.13074      -70.63751
##   yaw_dumbbell total_accel_dumbbell gyros_dumbbell_x gyros_dumbbell_y
## 1    -84.87394                   37                0            -0.02
## 2    -84.71065                   37                0            -0.02
##   gyros_dumbbell_z accel_dumbbell_x accel_dumbbell_y accel_dumbbell_z
## 1                0             -234               47             -271
## 2                0             -233               47             -269
##   magnet_dumbbell_x magnet_dumbbell_y magnet_dumbbell_z roll_forearm
## 1              -559               293               -65         28.4
## 2              -555               296               -64         28.3
##   pitch_forearm yaw_forearm total_accel_forearm gyros_forearm_x gyros_forearm_y
## 1         -63.9        -153                  36            0.03               0
## 2         -63.9        -153                  36            0.02               0
##   gyros_forearm_z accel_forearm_x accel_forearm_y accel_forearm_z
## 1           -0.02             192             203            -215
## 2           -0.02             192             203            -216
##   magnet_forearm_x magnet_forearm_y magnet_forearm_z classe
## 1              -17              654              476      A
## 2              -18              661              473      A
```

Factoring the `classe` variable:


```r
trainingClean$classe <- as.factor(trainingClean$classe)
```

# Partitioning the data

Now, let's divide this training data further into training and validation subsets.


```r
set.seed(3314)
inTrain <- createDataPartition(y=trainingClean$classe, p=.75, list=FALSE)
training <- trainingClean[inTrain,]
validation <- trainingClean[-inTrain,]

dim(training)
```

```
## [1] 14718    53
```

```r
dim(validation)
```

```
## [1] 4904   53
```

# Adding parallel processing

As training some of these models is likely to take a lot of time in computation, let's add parallel processing.


```r
library(parallel)
library(doParallel)
```

```
## Loading required package: foreach
```

```
## Loading required package: iterators
```

```r
cluster <- makeCluster(detectCores() - 1) # convention to leave 1 core for OS
registerDoParallel(cluster)
```

# Creating the models

Let's first set up the `trControl` variable for the `train` function.


```r
control <- trainControl(method='cv', number=5, verboseIter=F, allowParallel=T)
```

## Linear Discriminant Analysis


```r
# Model and prediction
mod_lda <- train(classe ~ ., data=training, method='lda')
pred_lda <- predict(mod_lda, validation)
conf_lda <- confusionMatrix(pred_lda, validation$classe)

# Confusion matrix
conf_lda
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1125  152   80   34   32
##          B   36  597   90   37  155
##          C  116  114  568  112   79
##          D  110   40   99  586  100
##          E    8   46   18   35  535
## 
## Overall Statistics
##                                           
##                Accuracy : 0.6956          
##                  95% CI : (0.6825, 0.7084)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.6151          
##                                           
##  Mcnemar's Test P-Value : < 2.2e-16       
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.8065   0.6291   0.6643   0.7289   0.5938
## Specificity            0.9151   0.9196   0.8960   0.9149   0.9733
## Pos Pred Value         0.7906   0.6525   0.5743   0.6267   0.8333
## Neg Pred Value         0.9224   0.9118   0.9267   0.9451   0.9141
## Prevalence             0.2845   0.1935   0.1743   0.1639   0.1837
## Detection Rate         0.2294   0.1217   0.1158   0.1195   0.1091
## Detection Prevalence   0.2902   0.1866   0.2017   0.1907   0.1309
## Balanced Accuracy      0.8608   0.7743   0.7802   0.8219   0.7835
```

## Decision trees


```r
mod_dt <- train(classe ~ ., data=training, method='rpart', trControl=control, tuneLength=6)
fancyRpartPlot(mod_dt$finalModel)
```

![](main_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

```r
# Prediction
pred_dt <- predict(mod_dt, validation)
conf_dt <- confusionMatrix(pred_dt, validation$classe)

# Confusion Matrix
conf_dt
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1247  406  400  344  118
##          B   28  284   24   11  125
##          C   82  104  370  130  102
##          D   37  155   61  319  154
##          E    1    0    0    0  402
## 
## Overall Statistics
##                                           
##                Accuracy : 0.5347          
##                  95% CI : (0.5206, 0.5487)
##     No Information Rate : 0.2845          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.3942          
##                                           
##  Mcnemar's Test P-Value : < 2.2e-16       
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.8939  0.29926  0.43275  0.39677  0.44617
## Specificity            0.6386  0.95247  0.89676  0.90073  0.99975
## Pos Pred Value         0.4958  0.60169  0.46954  0.43939  0.99752
## Neg Pred Value         0.9380  0.84995  0.88217  0.88392  0.88914
## Prevalence             0.2845  0.19352  0.17435  0.16395  0.18373
## Detection Rate         0.2543  0.05791  0.07545  0.06505  0.08197
## Detection Prevalence   0.5128  0.09625  0.16069  0.14804  0.08218
## Balanced Accuracy      0.7663  0.62586  0.66476  0.64875  0.72296
```

## Random Forests


```r
mod_rf <- train(classe ~ ., data=training, method='rf', trControl=control, tuneLength=6)

# Prediction
pred_rf <- predict(mod_rf, validation)
conf_rf <- confusionMatrix(pred_rf, validation$classe)

# Confusion Matrix
conf_rf
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1395    4    0    0    0
##          B    0  944    4    0    0
##          C    0    1  851    7    0
##          D    0    0    0  797    1
##          E    0    0    0    0  900
## 
## Overall Statistics
##                                          
##                Accuracy : 0.9965         
##                  95% CI : (0.9945, 0.998)
##     No Information Rate : 0.2845         
##     P-Value [Acc > NIR] : < 2.2e-16      
##                                          
##                   Kappa : 0.9956         
##                                          
##  Mcnemar's Test P-Value : NA             
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            1.0000   0.9947   0.9953   0.9913   0.9989
## Specificity            0.9989   0.9990   0.9980   0.9998   1.0000
## Pos Pred Value         0.9971   0.9958   0.9907   0.9987   1.0000
## Neg Pred Value         1.0000   0.9987   0.9990   0.9983   0.9998
## Prevalence             0.2845   0.1935   0.1743   0.1639   0.1837
## Detection Rate         0.2845   0.1925   0.1735   0.1625   0.1835
## Detection Prevalence   0.2853   0.1933   0.1752   0.1627   0.1835
## Balanced Accuracy      0.9994   0.9969   0.9967   0.9955   0.9994
```

## Models with their accuracy and out of sample error


```r
models <- c("Random Forest", "Linear Discriminant Analysis", "Decision Trees")
accuracy <- c(conf_rf$overall[1], conf_lda$overall[1], conf_dt$overall[1])
oos_err <- 1 - accuracy # out of sample error

data.frame(Accuracy = accuracy, Out_of_Sample_Error = oos_err, row.names = models)
```

```
##                               Accuracy Out_of_Sample_Error
## Random Forest                0.9965334         0.003466558
## Linear Discriminant Analysis 0.6955546         0.304445351
## Decision Trees               0.5346656         0.465334421
```
As we can see, the random forest model has the best accuracy or the least out of sample error. Let's use this model to predict the outcomes of the testing dataset.

# Predicting the testing set


```r
pred_finalmodel <- predict(mod_rf, testing)

pred_finalmodel
```

```
##  [1] B A B A A E D B A A B C B A E E A B B B
## Levels: A B C D E
```

# De-register parallel processing cluster


```r
stopCluster(cluster)
registerDoSEQ()
```
