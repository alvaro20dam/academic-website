---
title: Logistic Regression in R
author: Alvaro Gonzalez
date: '2025-02-15'
slug: logistic-regression-in-r
categories:
  - R
tags:
  - regression
  - R Markdown
subtitle: ''
summary: ''
authors: []
lastmod: '2025-02-15T07:57:20-04:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

# Decoding the Odds: Your Guide to Logistic Regression in R

Logistic regression.  The name itself might sound a bit intimidating, conjuring up images of complex equations and statistical jargon. But fear not!  While it's a powerful statistical tool, logistic regression is surprisingly accessible, especially when you have the right guide.  And that's exactly what this blog post aims to be: your friendly introduction to logistic regression in the R programming language.

Whether you're a seasoned data scientist looking for a refresher or a complete beginner just starting your journey into the world of predictive modeling, this post will break down the core concepts of logistic regression in a clear and concise way. We'll explore what it's used for, how it works under the hood (without getting too lost in the math!), and, most importantly, how to implement it in R.  We'll walk through a practical example, showing you how to prepare your data, build a logistic regression model, interpret the results, and even assess its performance.

So, if you're ready to unlock the power of predicting categorical outcomes – think yes/no, true/false, or even multiple categories – then you've come to the right place. Let's dive into the world of logistic regression in R and discover how it can help you make sense of your data!

## Logistic Regression in R: Predicting Customer Churn

### Load necessary libraries

``` r
library(dplyr)  # For data manipulation
library(ggplot2) # For plotting
library(caret)   # For model training and evaluation
```

Those three lines of code are how you load the R packages you've previously installed.  It's essential to run these `library()` commands after you've installed the packages (using `install.packages()`).

Here's a breakdown:

1. `library(dplyr)`: This line loads the dplyr package, making its functions available for use in your R session.  dplyr provides powerful tools for data manipulation, such as filter(), select(), mutate(), group_by(), and summarize().

2. `library(ggplot2`): This line loads the ggplot2 package, which is the most popular R package for creating beautiful and informative visualizations.  It's based on the "Grammar of Graphics" and allows you to build plots layer by layer.

3. `library(caret)`: This line loads the caret (Classification And REgression Training) package.  caret simplifies many machine learning tasks, including data splitting (like creating training and testing sets), preprocessing, model training, and evaluation.

### Load and Explore Data
Context
"Predict behavior to retain customers. You can analyze all relevant customer data and develop focused customer retention programs." [IBM Sample Data Sets]

Each row represents a customer, each column contains customer’s attributes described on the column Metadata.

``` r
churn_data <- read.csv('telco-customer-churn.csv')
```
The data set includes information about:

Customers who left within the last month – the column is called Churn
Services that each customer has signed up for – phone, multiple lines, internet, online security, online backup, device protection, tech support, and streaming TV and movies
Customer account information – how long they’ve been a customer, contract, payment method, paperless billing, monthly charges, and total charges
Demographic info about customers – gender, age range, and if they have partners and dependents

```
## 'data.frame':	7043 obs. of  21 variables:
##  $ customerID      : chr  "7590-VHVEG" "5575-GNVDE" "3668-QPYBK" "7795-CFOCW" ...
##  $ gender          : chr  "Female" "Male" "Male" "Male" ...
##  $ SeniorCitizen   : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Partner         : chr  "Yes" "No" "No" "No" ...
##  $ Dependents      : chr  "No" "No" "No" "No" ...
##  $ tenure          : int  1 34 2 45 2 8 22 10 28 62 ...
##  $ PhoneService    : chr  "No" "Yes" "Yes" "No" ...
##  $ MultipleLines   : chr  "No phone service" "No" "No" "No phone service" ...
##  $ InternetService : chr  "DSL" "DSL" "DSL" "DSL" ...
##  $ OnlineSecurity  : chr  "No" "Yes" "Yes" "Yes" ...
##  $ OnlineBackup    : chr  "Yes" "No" "Yes" "No" ...
##  $ DeviceProtection: chr  "No" "Yes" "No" "Yes" ...
##  $ TechSupport     : chr  "No" "No" "No" "Yes" ...
##  $ StreamingTV     : chr  "No" "No" "No" "No" ...
##  $ StreamingMovies : chr  "No" "No" "No" "No" ...
##  $ Contract        : chr  "Month-to-month" "One year" "Month-to-month" "One year" ...
##  $ PaperlessBilling: chr  "Yes" "No" "Yes" "No" ...
##  $ PaymentMethod   : chr  "Electronic check" "Mailed check" "Mailed check" "Bank transfer (automatic)" ...
##  $ MonthlyCharges  : num  29.9 57 53.9 42.3 70.7 ...
##  $ TotalCharges    : num  29.9 1889.5 108.2 1840.8 151.7 ...
##  $ Churn           : chr  "No" "No" "Yes" "No" ...
```

``` r
summary(churn_data)
```

```
##   customerID           gender          SeniorCitizen      Partner         
##  Length:7043        Length:7043        Min.   :0.0000   Length:7043       
##  Class :character   Class :character   1st Qu.:0.0000   Class :character  
##  Mode  :character   Mode  :character   Median :0.0000   Mode  :character  
##                                        Mean   :0.1621                     
##                                        3rd Qu.:0.0000                     
##                                        Max.   :1.0000                     
##                                                                           
##   Dependents            tenure      PhoneService       MultipleLines     
##  Length:7043        Min.   : 0.00   Length:7043        Length:7043       
##  Class :character   1st Qu.: 9.00   Class :character   Class :character  
##  Mode  :character   Median :29.00   Mode  :character   Mode  :character  
##                     Mean   :32.37                                        
##                     3rd Qu.:55.00                                        
##                     Max.   :72.00                                        
##                                                                          
##  InternetService    OnlineSecurity     OnlineBackup       DeviceProtection  
##  Length:7043        Length:7043        Length:7043        Length:7043       
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  TechSupport        StreamingTV        StreamingMovies      Contract        
##  Length:7043        Length:7043        Length:7043        Length:7043       
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  PaperlessBilling   PaymentMethod      MonthlyCharges    TotalCharges   
##  Length:7043        Length:7043        Min.   : 18.25   Min.   :  18.8  
##  Class :character   Class :character   1st Qu.: 35.50   1st Qu.: 401.4  
##  Mode  :character   Mode  :character   Median : 70.35   Median :1397.5  
##                                        Mean   : 64.76   Mean   :2283.3  
##                                        3rd Qu.: 89.85   3rd Qu.:3794.7  
##                                        Max.   :118.75   Max.   :8684.8  
##                                                         NA's   :11      
##     Churn          
##  Length:7043       
##  Class :character  
##  Mode  :character  
##                    
##                    
##                    
## 
```

``` r
# Convert variables to a factor (important for logistic regression)

column_to_preserve <- "customerID"  # Or a vector of names

churn_data <- churn_data %>%
  mutate(across(where(is.character) & !matches(column_to_preserve), factor))
```


``` r
churn_data$SeniorCitizen <- factor(churn_data$SeniorCitizen, levels = c(0, 1),
                                   labels = c("No", "Yes"))
```



``` r
sum(is.na(churn_data))
```

```
## [1] 11
```



