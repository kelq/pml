---
title: "PML - Assignment"
author: "KQ"
date: "25 October 2015"
output: html_document
---



# Background

Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. More information is available from the website here: http://groupware.les.inf.puc-rio.br/har (see the section on the Weight Lifting Exercise Dataset). 

# Data
The training data for this project are available here: 
https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

The test data are available here: 
https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv

```{r Load libraries}
setwd('D:/DS/09_PML')
library(caret)
library(randomForest)
library(rpart)

set.seed(3333)
```


### Read training data
``` {r}
# read training data
trainer <- read.csv("pml-training.csv", na.strings = c("", "NA"))

```

### Read test data
``` {r}
# read testing data
tester <- read.csv("pml-testing.csv", na.strings = c("", "NA"))
```


### Clean data

``` {r}
training <- trainer[,colSums(is.na(trainer)) == 0]
training <- training[,-c(1:7)]

testing <- tester[,colSums(is.na(tester)) == 0]
testing <- testing[, -c(1:7)]
```



### Split training data into training and validation set
``` {r}
temper <- createDataPartition(y = training$classe, p=0.6, list=FALSE)

train.data <- training[temper, ]
validate.data <- training[-temper, ]

```

### Build model
``` {r}
rf.model <- randomForest(classe ~. , data=train.data, method="class")
validate.out <- predict(rf.model, validate.data, type = "class")

confusionMatrix(validate.out, validate.data$classe)
```

### Apply to testing set

``` {r}
# Test results on subTesting data set:
confusionMatrix(validate.out, validate.data$classe)
```
