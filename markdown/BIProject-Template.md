Business Intelligence Project
================
<Adrianna Bitutu Ndubi>
<22 October 2023>

- [134126, Ndubi Adrianna](#student-details)
- [Setup Chunk](#setup-chunk)
- [Evaluation Metrics (classification and regression evaluation metrics)
  - [Loading the Dataset](#loading-the-dataset)
    - [Source:PimaIndiansDiabetes](#source)
    - [Reference:https://kaggle.com/datasets/salihacur/diabetes
    https://www.kaggle.com/datasets/willianoliveiragibin/healthcare-insurance/](#reference)

# Student Details

|                                              |     |
|----------------------------------------------|-----|
| 134126                      | …   |
| Ndubi Adrianna Bitutu                            | …   |
| Fearless Achievers                          | …   |
| **BI Project Group Name/ID (if applicable)** | …   |

# Setup Chunk

**Note:** the following KnitR options have been set as the global
defaults: <BR>
`knitr::opts_chunk$set(echo = TRUE, warning = FALSE, eval = TRUE, collapse = FALSE, tidy = TRUE)`.

More KnitR options are documented here
<https://bookdown.org/yihui/rmarkdown-cookbook/chunk-options.html> and
here <https://yihui.org/knitr/options/>.

# Evaluation Metrics

## Loading the Dataset

### Source:

The dataset that was used can be downloaded here: *\<https://www.kaggle.com/datasets/willianoliveiragibin/healthcare-insurance/ \>*
*\<https://kaggle.com/datasets/salihacur/diabetes \>*

##Setup Chunk
 1.c. Split the dataset ----
# Define a 75:25 train:test data split of the dataset.
# That is, 75% of the original data will be used to train the model and
# 25% of the original data will be used to test the model.
train_index <- createDataPartition(diabetes$diabetes,
                                   p = 0.75,
                                   list = FALSE)
diabetes_train <- diabetes[train_index, ]
diabetes_test <- diabetes[-train_index, ]

# Apply simple random sampling using the base::sample function to get
# 10 samples
train_index <- sample(1:dim(Insurance)[1], 5) # nolint: seq_linter.
Insurance_train <- Insurance[train_index, ]
Insurance_test <- Insurance[-train_index, ]

## 4.b. Train the Model ----
# We apply the 5-fold repeated cross validation resampling method
# with 3 repeats
train_control <- trainControl(method = "repeatedcv", number = 5, repeats = 3,
                              classProbs = TRUE,
                              summaryFunction = mnLogLoss)
set.seed(7)
# This creates a CART model. One of the parameters used by a CART model is "cp".
# "cp" refers to the "complexity parameter". It is used to impose a penalty to
# the tree for having too many splits. The default value is 0.01.
Insurance_model_cart <- train(age ~ ., data = Insurance, method = "rpart",
                         metric = "logLoss", trControl = train_control)

