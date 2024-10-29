library(rpart)
library(rpart.plot)
library(caret)
library(dplyr)

url <- "https://archive.ics.uci.edu/ml/machine-learning-databases/00222/bank.zip"
download.file(url, destfile = "bank.zip")
unzip("bank.zip")
data <- read.csv("bank-full.csv", sep = ";")

str(data)
summary(data)

data$y <- as.factor(data$y)

set.seed(123)
trainIndex <- createDataPartition(data$y, p = 0.7, list = FALSE)
trainData <- data[trainIndex, ]
testData <- data[-trainIndex, ]

tree_model <- rpart(y ~ ., data = trainData, method = "class")

rpart.plot(tree_model)

predictions <- predict(tree_model, testData, type = "class")

confusionMatrix(predictions, testData$y)

print(tree_model$variable.importance)
