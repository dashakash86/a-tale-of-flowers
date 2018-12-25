# a-tale-of-flowers
Apply statistical learning methods (Non-parametric, Parametric, Bayesian, Hypothesis Testing) to draw inferences 

## Abstract
This study was done to demonstrate the application of various statistical methods and tests using the iris dataset. Applying parametric, non-parametric, hypothesis testing, Bayesian, and chi-square approaches, this study provides interesting findings on the sepal width and various species of flowers in the 'iris' dataset. 

## Contents

1.	Introduction	
2.	Empirical CDF of Sepal Width	
3.	Non-parametric approach to estimate average 'sepal width'	
4.	Parametric Approach  	
5.	Hypothesis Testing on the mean
6.	Bayesian Analysis and Posterior Interval
7.	Chi-square Analysis across species
8.	Conclusions



## Introduction

*Dataset*: IRIS dataset by Fisher R.A in R to conduct our project study. The data set has a total of 150 observations and 5 variables. For the purpose of our study, the variables we are concerned with are ‘Sepal Width’ of 150 flowers across 3 different ‘Species’ – Setosa, Versicolor, and Virginica. 

## Methodology
* an empirical cumulative distribution based on the sample data, 
* followed by a non-parametric bootstrap to estimate standard errors and confidence intervals for mean Sepal Width,
* followed by parametric bootstrap (assuming that Sepal Width are normally distributed in the population) along with MLE of the mean  
* followed by a Wald Test to test a hypothesis on whether the mean Sepal Width of flowers is more than 3.0  
* then a Bayesian analysis (using prior and conjugal posterior) to verify the proportion of ‘large flowers’  
* and at last,  a Chi-square test to confirm the proportion of large flowers under each Species 


## Empirical CDF of Sepal Width
Assuming that the values we see in the data have equal probability mass (1/n), we calculated the CDF of that function. The empirical CDF for Sepal Width is shown below –

```
# Empirical Cumulative Distribution of 'Sepal Width'
sepal.width.ecdf <- ecdf(iris$Sepal.Width)
plot(sepal.width.ecdf)

```
![1](https://user-images.githubusercontent.com/44213899/50416782-4f38db00-07d7-11e9-989b-8fe6d6e96761.png)


## Non-parametric approach to estimate average 'sepal width'
Here we compute a non-parametric bootstrap to find the 3 types of C.Is for the mean Sepal Width.
*	We found the plug-in estimate X-bar = 3.05733 from the sample data which is the mean Sepal Width.
*	Then we bootstrapped to get the estimate of standard error for our estimate 
*	Standard error from the bootstrap came to around 0.03468 and we computed the three kinds of confidence intervals

```
# Non-parametric bootstrap to estimate average sepal width

  theta_hat <- mean(iris$Sepal.Width)
  theta <- function(x){ mean(x) }
  theta.NonPboot <- bootstrap(iris$Sepal.Width, 3200, theta)

   # Standard error from the botstrap vector
   theta.NonPboot_se <- sd(theta.NonPboot$thetastar)

    # Confidence intervals for our estimate
    nonPbootstrap_CI <- c(theta_hat-2*theta.NonPboot_se,theta_hat+2*theta.NonPboot_se)
    theta.NonPboot_se # 0.03468142

   ## Normal Confidence Interval (95%)
   normal.ci<-c(theta_hat-2*theta.NonPboot_se, theta_hat+2*theta.NonPboot_se)
   # (2.986420 3.128246)

   ## Pivotal Confidence Interval (95%)
   pivotal.ci<-c(2*theta_hat-quantile(theta.NonPboot$thetastar,0.975), 
                 2*theta_hat-quantile(theta.NonPboot$thetastar,0.025))
   # (2.987333, 3.127333)

   ## Quantile Confidence Interval (95%) 
   quantile.ci<-quantile(theta.NonPboot$thetastar, c(0.025, 0.975))
   # (2.987333, 3.127333)
```









