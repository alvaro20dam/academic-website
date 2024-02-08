---
title: Accidents analysis
author: Alvaro
date: '2024-01-10'
slug: new-post-dudes
categories:
  - R
tags:
  - plot
subtitle: "Welcome \U0001F44B We know that first impressions are important, so we've
  populated your new site with some initial content to help you get familiar with
  everything in no time."
summary: "Welcome \U0001F44B We know that first impressions are important, so we've
  populated your new site with some initial content to help you get familiar with
  everything in no time."
lastmod: '2024-01-10T16:07:43-04:00'
featured: false
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---


## The data

We’re going to explore data from the National Electronic Injury Surveillance System (NEISS), collected by the Consumer Product Safety Commission.

It’s an interesting dataset to explore because every one is already familiar with the domain, and each observation is accompanied by a short narrative that explains how the accident occurred.


```r
library(tidyverse)
```


```r
injuries <- read_tsv("C:/Users/admin/Desktop/R/R-todo/ER_injuries/neiss/injuries.tsv")
spec(injuries)
```

```
## cols(
##   trmt_date = col_date(format = ""),
##   age = col_double(),
##   sex = col_character(),
##   race = col_character(),
##   body_part = col_character(),
##   diag = col_character(),
##   location = col_character(),
##   prod_code = col_double(),
##   weight = col_double(),
##   narrative = col_character()
## )
```


```r
products <- read_tsv("C:/Users/admin/Desktop/R/R-todo/ER_injuries/neiss/products.tsv")
spec(products)
```

```
## cols(
##   prod_code = col_double(),
##   title = col_character()
## )
```


```r
population <- read_tsv("C:/Users/admin/Desktop/R/R-todo/ER_injuries/neiss/population.tsv")
spec(population)
```

```
## cols(
##   age = col_double(),
##   sex = col_character(),
##   population = col_double()
## )
```

## Exploration

We’ll start by looking at a product with an interesting story: 649, “toilets”. First, we’ll pull out the injuries associated with this product:


```r
selected <- injuries %>% filter(prod_code == 649)
nrow(selected)
```

```
## [1] 2993
```

Next, we’ll perform some basic summaries looking at the location, body part, and diagnosis of toilet-related injuries.


```r
selected %>% count(location, wt = weight, sort = TRUE)
```

```
## # A tibble: 6 × 2
##   location                         n
##   <chr>                        <dbl>
## 1 Home                       99603. 
## 2 Other Public Property      18663. 
## 3 Unknown                    16267. 
## 4 School                       659. 
## 5 Street Or Highway             16.2
## 6 Sports Or Recreation Place    14.8
```


```r
selected %>% count(body_part, wt = weight, sort = TRUE)
```

```
## # A tibble: 24 × 2
##    body_part        n
##    <chr>        <dbl>
##  1 Head        31370.
##  2 Lower Trunk 26855.
##  3 Face        13016.
##  4 Upper Trunk 12508.
##  5 Knee         6968.
##  6 N.S./Unk     6741.
##  7 Lower Leg    5087.
##  8 Shoulder     3590.
##  9 All Of Body  3438.
## 10 Ankle        3315.
## # ℹ 14 more rows
```


```r
selected %>% count(diag, wt = weight, sort = TRUE)
```

```
## # A tibble: 20 × 2
##    diag                        n
##    <chr>                   <dbl>
##  1 Other Or Not Stated   32897. 
##  2 Contusion Or Abrasion 22493. 
##  3 Inter Organ Injury    21525. 
##  4 Fracture              21497. 
##  5 Laceration            18734. 
##  6 Strain, Sprain         7609. 
##  7 Dislocation            2713. 
##  8 Hematoma               2386. 
##  9 Avulsion               1778. 
## 10 Nerve Damage           1091. 
## 11 Poisoning               928. 
## 12 Concussion              822. 
## 13 Dental Injury           199. 
## 14 Hemorrhage              167. 
## 15 Crushing                114. 
## 16 Dermat Or Conj           84.2
## 17 Burns, Not Spec          67.2
## 18 Puncture                 67.2
## 19 Burns, Thermal           34.0
## 20 Burns, Scald             17.0
```

As you might expect, injuries involving toilets most often occur at home. The most common body parts involved possibly suggest that these are falls (since the head and face are not usually involved in routine toilet usage), and the diagnoses seem rather varied.

We can also explore the pattern across age and sex. We have enough data here that a table is not that useful, and so I make a plot, Fig..., that makes the patterns more obvious.


```r
summary <- selected %>% 
  count(age, sex, wt = weight)
summary
```

```
## # A tibble: 208 × 3
##      age sex          n
##    <dbl> <chr>    <dbl>
##  1     0 female    4.76
##  2     0 male     14.3 
##  3     1 female  253.  
##  4     1 male    231.  
##  5     2 female  438.  
##  6     2 male    632.  
##  7     3 female  381.  
##  8     3 male   1004.  
##  9     4 female  261.  
## 10     4 male    843.  
## # ℹ 198 more rows
```

```r
summary %>% 
  ggplot(aes(age, n, colour = sex)) + 
  geom_line() + 
  labs(y = "Estimated number of injuries")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

We see a spike for young boys peaking at age 3, and then an increase (particularly for women) starting around middle age, and a gradual decline after age 80.

One problem with interpreting this pattern is that we know that there are fewer older people than younger people, so the population available to be injured is smaller. We can control for this by comparing the number of people injured with the total population and calculating an injury rate. Here I use a rate per 10,000.


```r
summary <- selected %>% 
  count(age, sex, wt = weight) %>% 
  left_join(population, by = c("age", "sex")) %>% 
  mutate(rate = n / population * 1e4)

summary
```

```
## # A tibble: 208 × 5
##      age sex          n population   rate
##    <dbl> <chr>    <dbl>      <dbl>  <dbl>
##  1     0 female    4.76    1924145 0.0247
##  2     0 male     14.3     2015150 0.0708
##  3     1 female  253.      1943534 1.30  
##  4     1 male    231.      2031718 1.14  
##  5     2 female  438.      1965150 2.23  
##  6     2 male    632.      2056625 3.07  
##  7     3 female  381.      1956281 1.95  
##  8     3 male   1004.      2050474 4.90  
##  9     4 female  261.      1953782 1.33  
## 10     4 male    843.      2042001 4.13  
## # ℹ 198 more rows
```


```r
summary %>% 
  ggplot(aes(age, rate, colour = sex)) + 
  geom_line(na.rm = TRUE) + 
  labs(y = "Injuries per 10,000 people")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="672" />

[Try this app](https://alvarogonzalez.shinyapps.io/ER_injuries/ "injuriesApp")
