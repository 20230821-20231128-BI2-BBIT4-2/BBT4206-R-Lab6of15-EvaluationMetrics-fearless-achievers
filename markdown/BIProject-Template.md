Business Intelligence Project
================
<Adrianna Bitutu Ndubi>
<22 October 2023>

- [134126, Ndubi Adrianna](#student-details)
- [Setup Chunk](#setup-chunk)
- [Understanding the Dataset (Resampling Methods
  (EDA))](#understanding-the-dataset-exploratory-data-analysis-eda)
  - [Loading the Dataset](#loading-the-dataset)
    - [Source:PimaIndiansDiabetes](#source)
    - [Reference:Smith, J.W., Everhart, J.E., Dickson, W.C., Knowler, W.C., & Johannes, R.S. (1988). Using the ADAP learning algorithm to forecast the onset of diabetes mellitus. ](#reference)

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

# Resampling Methods)

## Loading the Dataset

### Source:

The dataset that was used can be downloaded here: *\<Smith, J.W., Everhart, J.E., Dickson, W.C., Knowler, W.C., & Johannes, R.S. (1988). Using the ADAP learning algorithm to forecast the onset of diabetes mellitus. \>*

### Reference:

*\<Cite the dataset here using APA\>  
Refer to the APA 7th edition manual for rules on how to cite datasets:
<https://apastyle.apa.org/style-grammar-guidelines/references/examples/data-set-references>*

``` ### 2.a. OPTION 1: naiveBayes() function in the e1071 package ----
# The "naiveBayes()" function (case sensitive) in the "e1071" package
# is less sensitive to missing values hence all the features (variables
# /attributes) are considered as independent variables that have an effect on
# the dependent variable (stock).

PimaIndiansDiabetes_model_nb_e1071 <- # nolint
  e1071::naiveBayes(stock ~ quarter + date + open + high + low + close +
                      volume + percent_change_price +
                      percent_change_volume_over_last_wk +
                      previous_weeks_volume + next_weeks_open +
                      next_weeks_close + percent_change_next_weeks_price +
                      days_to_next_dividend + percent_return_next_dividend,
                    data = PimaIndiansDiabetes_train)
 
``` ### 3.a. Test the trained e1071 Naive Bayes model using the testing dataset ----
predictions_nb_e1071 <-
  predict(PimaIndiansDiabetes_model_nb_e1071,
          PimaIndiansDiabetes_test[, c("quarter", "date", "open", "high",
                                     "low", "close", "volume",
                                     "percent_change_price",
                                     "percent_change_volume_over_last_wk",
                                     "previous_weeks_volume", "next_weeks_open",
                                     "next_weeks_close",
                                     "percent_change_next_weeks_price",
                                     "days_to_next_dividend",
                                     "percent_return_next_dividend")])
                                     
## 4. View the Results ----
### 4.a. e1071 Naive Bayes model and test results using a confusion matrix ----
# Please watch the following video first: https://youtu.be/Kdsp6soqA7o
print(predictions_nb_e1071)
caret::confusionMatrix(predictions_nb_e1071,
                       PimaIndiansDiabetes_test[, c("quarter", "date", "open",
                                                  "high", "low", "close",
                                                  "volume",
                                                  "percent_change_price",
                                                  "percent_change_volume_over_last_wk", # nolint
                                                  "previous_weeks_volume",
                                                  "next_weeks_open",
                                                  "next_weeks_close",
                                                  "percent_change_next_weeks_price", # nolint
                                                  "days_to_next_dividend",
                                                  "percent_return_next_dividend", # nolint
                                                  "stock")]$stock)
plot(table(predictions_nb_e1071,
           PimaIndiansDiabetes_test[, c("quarter", "date", "open", "high", "low",
                                      "close", "volume", "percent_change_price",
                                      "percent_change_volume_over_last_wk",
                                      "previous_weeks_volume",
                                      "next_weeks_open", "next_weeks_close",
                                      "percent_change_next_weeks_price",
                                      "days_to_next_dividend",
                                      "percent_return_next_dividend",
                                      "stock")]$stock))
