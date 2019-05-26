# Assignments
---
output:
  html_document: default
  pdf_document: default
  word_document: default
---
output: rmarkdown::github_document

#Mlashleeadams Assignment 2

# Determine which characteristic might be related to home ownership rates

# I have selected median household income and college graduate

# 1. Calculate the mean of the outcome

Load libraries

```{r  include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
library(tidyverse)
library(plotly)
library(Metrics)
```

## Load Dataset
United States counties data put together by the Census Bureau that summarizes the characteristics of the 3,088 counties in the United States. 

```{r data}
load("pd.Rdata")
```

# predictive factor
```{r mean}
##Mean of median household income 2008-2012
pd%>%summarize(mean_hhinc=mean(median_hh_inc,na.rm=TRUE))
```

# Question 2. Create a new variable that consists of the mean of median household income
```{r new variable}
## Create a rank variable for income 
pd<-pd%>%mutate(hhinc_rank=rank(median_hh_inc))
```

# Question 3. Calculate a summary measure of the errors for each observation

```{r error}
##Variable for error
pd<-pd%>%mutate(e2=median_hh_inc-mean_hhinc)
## RMSE
rmse_uncond_mean<-rmse(pd$median_hh_inc,pd$mean_hhinc)
rmse_uncond_mean
```


# 4. Calculate the mean of the outcome at levels of a predictor variable.
# I selected person per household as a predictor
```{r condtl_mean_single}
##Condtional Average across a single variable
## Create a variable for quartiles of college education
pd<-pd%>%mutate(household_level=ntile(person_per_hh,4))
table(pd$household_level)
pd<-pd%>%group_by(household_level)%>% ## Group by predictor
  ##Calculate mean at each level of predictor
  mutate(pred_income_household=mean(median_hh_inc))%>% 
  ## Ungroup
  ungroup()%>% 
  #Rank by prediction, with ties sorted randomly
  mutate(pred_income_household_rank=rank(pred_income_household,ties.method="random"))
```

# 5. Use these conditional means as a prediction: for every county, use the conditional mean to provide a ‘’best guess” as to that county’s level of the outcome.

## New Variable Home Ownership Rate
```{r}
## Create a variable for quartiles of home ownership
pd<-pd%>%mutate(homeown_rate_level=ntile(homeown_rate,4))
```

```{r}
pd%>%group_by(homeown_rate_level)%>% ## Group by predictor
  ##Calculate mean at each level of predictor
  summarise(pred_income_homeown_rate=mean(median_hh_inc))
```


# 6. Calculate a summary measure of the error in your predictions.

```{r}
rmse_cond_mean_two<-rmse(pd$median_hh_inc,pd$pred_income_household_and_homeown)
rmse_cond_mean_two
```
