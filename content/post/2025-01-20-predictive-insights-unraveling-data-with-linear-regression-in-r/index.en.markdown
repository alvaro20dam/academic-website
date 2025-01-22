---
title: 'Predictive Insights: Unraveling Data with Linear Regression in R'
author: Alvaro Gonz
date: '2025-01-20'
slug: predictive-insights-unraveling-data-with-linear-regression-in-r
categories:
  - R
tags:
  - Academic
  - Statistics
subtitle: ''
summary: ''
authors: []
lastmod: '2025-01-20T19:48:10-04:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
output: html_document
---

## Project Introduction

In an era dominated by data, the ability to derive meaningful insights from complex datasets is more crucial than ever. Linear regression stands at the forefront of statistical modeling, offering a powerful yet accessible method for predicting outcomes and understanding relationships between variables. Imagine being able to forecast housing prices, assess the impact of advertising budgets on sales, or even determine factors influencing student performance—all through the lens of a simple mathematical equation. This project delves into the art and science of linear regression in R, illuminating its potential to turn raw data into actionable knowledge.

Join us as we explore the intricacies of building and evaluating linear regression models, while equipping you with the skills to harness this technique for your own data analysis challenges. Whether you're a seasoned data scientist or curious about the analytics world, this journey will unlock the secrets of linear regression, making it an invaluable tool in your analytical toolkit. Let’s embark on this exciting exploration into the transformative power of data-driven decision-making!

### Overview of Linear Regression

Linear regression is a statistical method used to model the relationship between a dependent variable and one or more independent variables by fitting a linear equation to observed data. The simplest form, simple linear regression, involves two variables: the dependent variable (y) is predicted based on one independent variable (x) using the formula `\(y = mx + b\)`, where m represents the slope of the line and b represents the y-intercept. In multiple linear regression, multiple independent variables are included in the model, extending the formula to `\(y = b0 + b1x1 + b2x2 + ... + bkxk\)`.

The key goals of linear regression include understanding the strength and direction of relationships between variables, making predictions, and assessing the importance of various predictors. It relies on assumptions such as linearity, independence, homoscedasticity (constant variance), and normality of errors. The effectiveness of a linear regression model is often evaluated using metrics like R-squared, which indicates the proportion of variance explained by the model, and residual analysis to check for violations of assumptions.

### Importance and Applications in Data Science

Linear regression is crucial in data science due to its simplicity and interpretability, making it a foundational tool for predictive modeling. It helps identify relationships between variables, assess correlations, and make forecasts based on historical data. 

**Applications include:**  
1. **Economics:** Modeling consumer behavior and forecasting economic trends.  
2. **Marketing:** Analyzing advertising effectiveness and customer preferences.  
3. **Healthcare:** Predicting outcomes based on patient data and treatment effects.  
4. **Finance:** Risk assessment and stock price prediction.  
5. **Real Estate:** Estimating property prices based on various features.

Linear regression serves as a valuable starting point for more complex models, enabling data scientists to gain insights, simplify predictive tasks, and communicate findings effectively.

### Objectives of the Project



## Loading the dataset




``` r
str(data)
```

```
## 'data.frame':	500 obs. of  8 variables:
##  $ Email               : chr  "mstephenson@fernandez.com" "hduke@hotmail.com" "pallen@yahoo.com" "riverarebecca@gmail.com" ...
##  $ Address             : chr  "835 Frank Tunnel\nWrightmouth, MI 82180-9605" "4547 Archer Common\nDiazchester, CA 06566-8576" "24645 Valerie Unions Suite 582\nCobbborough, DC 99414-7564" "1414 David Throughway\nPort Jason, OH 22070-1220" ...
##  $ Avatar              : chr  "Violet" "DarkGreen" "Bisque" "SaddleBrown" ...
##  $ Avg..Session.Length : num  34.5 31.9 33 34.3 33.3 ...
##  $ Time.on.App         : num  12.7 11.1 11.3 13.7 12.8 ...
##  $ Time.on.Website     : num  39.6 37.3 37.1 36.7 37.5 ...
##  $ Length.of.Membership: num  4.08 2.66 4.1 3.12 4.45 ...
##  $ Yearly.Amount.Spent : num  588 392 488 582 599 ...
```
This data contains information about customer characteristics (email, address, avatar color), their engagement metrics (average session length, time spent on app and website), and their purchase behavior (length of membership and yearly amount spent).


``` r
summary(data)
```

```
##     Email             Address             Avatar          Avg..Session.Length
##  Length:500         Length:500         Length:500         Min.   :29.53      
##  Class :character   Class :character   Class :character   1st Qu.:32.34      
##  Mode  :character   Mode  :character   Mode  :character   Median :33.08      
##                                                           Mean   :33.05      
##                                                           3rd Qu.:33.71      
##                                                           Max.   :36.14      
##   Time.on.App     Time.on.Website Length.of.Membership Yearly.Amount.Spent
##  Min.   : 8.508   Min.   :33.91   Min.   :0.2699       Min.   :256.7      
##  1st Qu.:11.388   1st Qu.:36.35   1st Qu.:2.9304       1st Qu.:445.0      
##  Median :11.983   Median :37.07   Median :3.5340       Median :498.9      
##  Mean   :12.052   Mean   :37.06   Mean   :3.5335       Mean   :499.3      
##  3rd Qu.:12.754   3rd Qu.:37.72   3rd Qu.:4.1265       3rd Qu.:549.3      
##  Max.   :15.127   Max.   :40.01   Max.   :6.9227       Max.   :765.5
```



``` r
library(ggplot2)

# Is there a correlation between Time on Website & Yearly Amount Spent?
ggplot(data, aes(x=Time.on.Website, y=Yearly.Amount.Spent)) + 
  geom_point(colour="orange") + 
  ggtitle("Time on website against vs Yearly amount spent") + 
  xlab("Time on Website") +
  ylab("Yearly Amount Spent")
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-4-1.png" width="672" />

This R code snippet uses the `ggplot2` library to create a scatter plot visualizing the relationship between the average session length of website visits and the yearly amount spent by customers.

Here's a breakdown of the code:

1. **`ggplot(data, aes(x=Avg..Session.Length, y=Yearly.Amount.Spent))`**:
   - This line initiates the ggplot object.
   - `data`: This should be the name of your data frame containing the relevant columns.
   - `aes(x=Avg..Session.Length, y=Yearly.Amount.Spent)`: This specifies the aesthetics of the plot:
     - `x=Avg..Session.Length`: The x-axis variable representing the average session length.
     - `y=Yearly.Amount.Spent`: The y-axis variable representing the yearly amount spent.

2. **`geom_point(colour="orange")`**:
   - This adds points to the plot.
   - `colour="orange"`: Sets the color of the points to orange.

3. **`ggtitle("Average session length against vs Yearly amount spent")`**:
   - Sets the title of the plot.

4. **`xlab("Time on Website")`**:
   - Sets the label for the x-axis.

5. **`ylab("Yearly Amount Spent")`**:
   - Sets the label for the y-axis.

This plot will help you visually explore whether there's a correlation between the time customers spend on your website and the amount they spend annually. You might observe patterns like:

- **Positive correlation:** Customers who spend more time on the website tend to spend more money.
- **Negative correlation:** Customers who spend less time on the website tend to spend more money (which might be unexpected and require further investigation).
- **No correlation:** There's no apparent relationship between session length and yearly spending.

By analyzing this plot, you can gain insights into customer behavior and potentially optimize your website or marketing strategies to encourage more spending.



``` r
# Is there a correlation between Avg Session Length & Yearly Amount Spent?
ggplot(data, aes(x=Avg..Session.Length, y=Yearly.Amount.Spent)) + 
  geom_point(colour="orange") +
  ggtitle("Average session length against vs Yearly amount spent") + 
  xlab("Time on Website") +
  ylab("Yearly Amount Spent")
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-5-1.png" width="672" />

This R code snippet uses the `ggplot2` library to create a scatter plot visualizing the relationship between the average session length of website visits and the yearly amount spent by customers.

Here's a breakdown of the code:

1. **`ggplot(data, aes(x=Avg..Session.Length, y=Yearly.Amount.Spent))`**:
   - This line initiates the ggplot object.
   - `data`: This should be the name of your data frame containing the relevant columns.
   - `aes(x=Avg..Session.Length, y=Yearly.Amount.Spent)`: This specifies the aesthetics of the plot:
     - `x=Avg..Session.Length`: The x-axis variable representing the average session length.
     - `y=Yearly.Amount.Spent`: The y-axis variable representing the yearly amount spent.

2. **`geom_point(colour="orange")`**:
   - This adds points to the plot.
   - `colour="orange"`: Sets the color of the points to orange.

3. **`ggtitle("Average session length against vs Yearly amount spent")`**:
   - Sets the title of the plot.

4. **`xlab("Time on Website")`**:
   - Sets the label for the x-axis.

5. **`ylab("Yearly Amount Spent")`**:
   - Sets the label for the y-axis.

This plot will help you visually explore whether there's a correlation between the time customers spend on your website and the amount they spend annually. You might observe patterns like:

- **Positive correlation:** Customers who spend more time on the website tend to spend more money.
- **Negative correlation:** Customers who spend less time on the website tend to spend more money (which might be unexpected and require further investigation).
- **No correlation:** There's no apparent relationship between session length and yearly spending.

By analyzing this plot, you can gain insights into customer behavior and potentially optimize your website or marketing strategies to encourage more spending.



``` r
# pairplot of all continuous variables --> 
#### length of membership seems the most correlated
pairs(data[c("Avg..Session.Length", 
             "Time.on.App", 
             "Time.on.Website", 
             "Length.of.Membership",
             "Yearly.Amount.Spent")],
      col = "orange",
      pch = 16,
      labels = c("Avg Session Length", 
                 "Time on App", 
                 "Time on website",
                 "Length of Membership",
                 "Yearly spent"),
      main = "Pairplot of variables")
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-6-1.png" width="672" />


``` r
# is the variable normally distributed?
hist(data$Length.of.Membership)
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-7-1.png" width="672" />

``` r
# with ggplot
ggplot(data, aes(x=Length.of.Membership)) + 
  geom_histogram(
    color= "white", 
    fill="orange",
    binwidth = 0.5)
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-7-2.png" width="672" />

``` r
# check distribution with boxplot
boxplot(data$Length.of.Membership)
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-7-3.png" width="672" />

``` r
# with ggplot
ggplot(data, aes(x=Length.of.Membership)) + 
  geom_boxplot(
    fill="orange",
  )
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-7-4.png" width="672" />

``` r
attach(data)

lm.fit1 <- lm(Yearly.Amount.Spent~Length.of.Membership)

summary(lm.fit1)
```

```
## 
## Call:
## lm(formula = Yearly.Amount.Spent ~ Length.of.Membership)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -125.975  -29.032   -0.494   33.033  147.777 
## 
## Coefficients:
##                      Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           272.400      7.675   35.49   <2e-16 ***
## Length.of.Membership   64.219      2.090   30.72   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 46.66 on 498 degrees of freedom
## Multiple R-squared:  0.6546,	Adjusted R-squared:  0.6539 
## F-statistic: 943.9 on 1 and 498 DF,  p-value: < 2.2e-16
```

``` r
# --> p value is below significance level, so the variable Length of 
#     membership is significant.
# --> beta_1 is 64.219, which means that an increase in the variable value
#     causes an increase in the target variable.

plot(Yearly.Amount.Spent~Length.of.Membership)
abline(lm.fit1, col="red")
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-8-1.png" width="672" />


``` r
qqnorm(residuals(lm.fit1))
qqline(residuals(lm.fit1), col="red")
```

<img src="{{< blogdown/postref >}}index.en_files/figure-html/unnamed-chunk-9-1.png" width="672" />

``` r
shapiro.test(residuals(lm.fit1))
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  residuals(lm.fit1)
## W = 0.99756, p-value = 0.6837
```

``` r
# --> the p value is > 0.05 so Ho cannot be rejected. The residuals 
#     distribution is normal.
# --> normality of residuals is an assumption of the linear model.
#     this means that the chosen model works correctly.
```


``` r
# create a random training and a testing set
set.seed(1)
row.number <- sample(1:nrow(data), 0.8*nrow(data))

train <- data[row.number,]
test <- data[-row.number,]

# estimate the linear fit with the training set
lm.fit0.8 <- lm(Yearly.Amount.Spent~Length.of.Membership, data=train)
summary(lm.fit0.8)
```

```
## 
## Call:
## lm(formula = Yearly.Amount.Spent ~ Length.of.Membership, data = train)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -124.810  -29.274   -2.219   31.482  149.107 
## 
## Coefficients:
##                      Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           271.853      8.691   31.28   <2e-16 ***
## Length.of.Membership   64.073      2.355   27.21   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 47.14 on 398 degrees of freedom
## Multiple R-squared:  0.6503,	Adjusted R-squared:  0.6494 
## F-statistic: 740.2 on 1 and 398 DF,  p-value: < 2.2e-16
```

``` r
# predict on testing set
prediction0.8 <- predict(lm.fit0.8, newdata = test)
err0.8 <- prediction0.8 - test$Yearly.Amount.Spent
rmse <- sqrt(mean(err0.8^2))
mape <- mean(abs(err0.8/test$Yearly.Amount.Spent))

c(RMSE=rmse,mape=mape,R2=summary(lm.fit0.8)$r.squared) # to print the 3 parameters
```

```
##        RMSE        mape          R2 
## 44.78105782  0.07692126  0.65032683
```


``` r
attach(data)
```

```
## The following objects are masked from data (pos = 3):
## 
##     Address, Avatar, Avg..Session.Length, Email, Length.of.Membership,
##     Time.on.App, Time.on.Website, Yearly.Amount.Spent
```

``` r
lm.fit <- lm(Yearly.Amount.Spent~Avg..Session.Length +
                                    Time.on.App + 
                                    Time.on.Website +
                                    Length.of.Membership)

summary(lm.fit)
```

```
## 
## Call:
## lm(formula = Yearly.Amount.Spent ~ Avg..Session.Length + Time.on.App + 
##     Time.on.Website + Length.of.Membership)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -30.4059  -6.2191  -0.1364   6.6048  30.3085 
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)          -1051.5943    22.9925 -45.736   <2e-16 ***
## Avg..Session.Length     25.7343     0.4510  57.057   <2e-16 ***
## Time.on.App             38.7092     0.4510  85.828   <2e-16 ***
## Time.on.Website          0.4367     0.4441   0.983    0.326    
## Length.of.Membership    61.5773     0.4483 137.346   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 9.973 on 495 degrees of freedom
## Multiple R-squared:  0.9843,	Adjusted R-squared:  0.9842 
## F-statistic:  7766 on 4 and 495 DF,  p-value: < 2.2e-16
```

``` r
# --------------
# findings :
# 3 of the 4 variables studied seem to have an positive impact on the
# response variable. the most important remains length of membership, with 
# a coefficient 1.5 and 2.4 higher than Time on App and Avg Session Length 
# respectively. 
# Time on website seems to have little impact in the response. 
```

``` r
# create a random training and a testing set
set.seed(1)
row.number <- sample(1:nrow(data), 0.8*nrow(data))

train <- data[row.number,]
test <- data[-row.number,]

# estimate the linear fit with the training set
multi.lm.fit0.8 <- lm(Yearly.Amount.Spent~Avg..Session.Length +
                        Time.on.App + 
                        Time.on.Website +
                        Length.of.Membership, 
                      data=train)
summary(multi.lm.fit0.8)
```

```
## 
## Call:
## lm(formula = Yearly.Amount.Spent ~ Avg..Session.Length + Time.on.App + 
##     Time.on.Website + Length.of.Membership, data = train)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -30.042  -6.496  -0.084   6.960  30.045 
## 
## Coefficients:
##                        Estimate Std. Error t value Pr(>|t|)    
## (Intercept)          -1044.3952    26.6175 -39.237   <2e-16 ***
## Avg..Session.Length     25.6921     0.5198  49.425   <2e-16 ***
## Time.on.App             38.9626     0.5171  75.354   <2e-16 ***
## Time.on.Website          0.2212     0.5065   0.437    0.663    
## Length.of.Membership    61.2990     0.5156 118.898   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 10.27 on 395 degrees of freedom
## Multiple R-squared:  0.9835,	Adjusted R-squared:  0.9834 
## F-statistic:  5897 on 4 and 395 DF,  p-value: < 2.2e-16
```

``` r
# predict on testing set
prediction.multi0.8 <- predict(multi.lm.fit0.8, newdata = test)
err0.8 <- prediction.multi0.8 - test$Yearly.Amount.Spent
rmse <- sqrt(mean(err0.8^2))
mape <- mean(abs(err0.8/test$Yearly.Amount.Spent))

c(RMSE=rmse,mape=mape,R2=summary(lm.fit0.8)$r.squared) # to print the 3 parameters
```

```
##       RMSE       mape         R2 
## 8.76502797 0.01475183 0.65032683
```

``` r
# --------
# findings
# by using a multiple linear model, we have created a much more accurate
# predictor of the response variable. R2 went from 0.65 to 0.98
# and the RSE went from 47.14 to 9.97 dollars.
```
