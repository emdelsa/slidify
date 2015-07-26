---
title       : Wine Quality Prediction
subtitle    : Developing Data Products Course Project
author      : Emilio Delgado
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

<style>
em {
  font-style: italic
}
</style>




## The Dataset

![logoUCI](assets/img/logoUCI.gif)

The data for this project comes from the Wine Quality Data Set at the UCI Machine Learning Repository. There are two datasets, corresponding to red and white variants of the Portuguese "Vinho Verde" wine.

Credit:
P. Cortez, A. Cerdeira, F. Almeida, T. Matos and J. Reis. 
_Modeling wine preferences by data mining from physicochemical properties_. 
In *Decision Support Systems*, Elsevier, 47(4):547-553, 2009.

Data is loaded with the following commands:


```r
url<- "http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/"
dataWhite <- read.csv(paste(url,"winequality-white.csv",sep=""),sep=";")
dataRed <- read.csv(paste(url,"winequality-red.csv",sep=""),sep=";")
```

Libraries needed for the analysis: caret



---

## Data attributes


```r
names(dataWhite)
```

```
##  [1] "fixed.acidity"        "volatile.acidity"     "citric.acid"         
##  [4] "residual.sugar"       "chlorides"            "free.sulfur.dioxide" 
##  [7] "total.sulfur.dioxide" "density"              "pH"                  
## [10] "sulphates"            "alcohol"              "quality"
```

In both datasets, the output label is "quality", the rest of the variables are predictors. After some analysis, We have selected two predictors: volatile.acidity and alcohol. The "quality" variable is an integer, ordered quantity representing the wine quality degree which has been compressed into two categories: GOOD for quality>5 and REGULAR/BAD for the rest:

```r
dataWhite$q <- as.factor(dataWhite$quality>5)
dataRed$q <- as.factor(dataRed$quality>5)
```

---


## Data Partition & Model Learning

We now create the training and testing data sets:


```r
intrainW<-createDataPartition(dataWhite$q,p=0.7,list=FALSE)
trainW <- dataWhite[intrainW,]; testW <- dataWhite[-intrainW,]
intrainR<-createDataPartition(dataRed$q,p=0.7,list=FALSE)
trainR <- dataRed[intrainR,] ; testR <- dataRed[-intrainR,]
```

We will use logistic regression for our two models (one per wine variety):

```r
modelW <- train(q~volatile.acidity+alcohol,data=trainW,method="glm",family=binomial)
predW <- predict(modelW,newdata=testW) ; confW <- confusionMatrix(predW,testW$q)
modelR <- train(q~volatile.acidity+alcohol,data=trainR,method="glm",family=binomial)
predR <- predict(modelR,newdata=testR) ; confR <- confusionMatrix(predR,testR$q)
```

---

## The Model 

Expected accuracy:

```r
confW$overall[1]
```

```
##  Accuracy 
## 0.7440436
```

```r
confR$overall[1]
```

```
##  Accuracy 
## 0.7202505
```







