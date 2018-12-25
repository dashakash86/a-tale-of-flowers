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

    # Normal Confidence Interval (95%)
    normal.ci<-c(theta_hat-2*theta.NonPboot_se, theta_hat+2*theta.NonPboot_se)
    # (2.986420 3.128246)

    ## Pivotal Confidence Interval (95%)
    pivotal.ci<-c(2*theta_hat-quantile(theta.NonPboot$thetastar,0.975), 
                 2*theta_hat-quantile(theta.NonPboot$thetastar,0.025))
    # (2.987333, 3.127333)

    # Quantile Confidence Interval (95%) 
    quantile.ci<-quantile(theta.NonPboot$thetastar, c(0.025, 0.975))
    # (2.987333, 3.127333)
```
![1 1](https://user-images.githubusercontent.com/44213899/50417595-354dc700-07dc-11e9-9482-e47414aa0a39.png)


## Parametric Approach
We were able to determine, from our academic research, that the sepal width in the flower population of the species concerned does follow a normal distribution. Also when we inspected the histogram for Sepal Width, we found that it was reasonably normal, as shown below. 

![2](https://user-images.githubusercontent.com/44213899/50417976-f882cf80-07dd-11e9-84ed-61078a1c55ec.png)

Therefore, in this section we did an MLE for the mean of Sepal Width, followed by a parametric bootstrap to determine standard error and a 95% confidence interval. 

```
# Parametric method for estimation of mean (MLE and Bootstrap)

  set.seed(123)
  m1 <- mle2(iris$Sepal.Width ~ dnorm(mean = mu, sd = sd), 
             data = as.data.frame(iris$Sepal.Width), start = list(mu = 1, sd = 2)) 
  coefficients(m1)

  #Coefficients:
  #  Estimate   Std. Error   z value      Pr(z)    
  #  3.057333   0.035469    86.196   < 2.2e-16 ***
  #  0.434410   0.025081    17.320   < 2.2e-16 ***

  # Inspection of the asymptotic distribution of 'sepal width' with MLE as mean
  par(mfrow=c(1,2))
  hist(rnorm(1000, mle_mu, mle_sd), breaks=20)
  hist(iris$Sepal.Width, breaks = 20)


  #Parametric Boostrap
  B <- 3000
  n <- length(iris$Sepal.Width)
  theta.hat <- function(x){ mean(x) }
   
  # Use of 'replicate' function instead of 'bootstrap' function 
  boot.theta.hat <- replicate(B, theta.hat(rnorm(n, mean = mle_mu, sd = mle_sd) ))
  para_se <- sd(boot.theta.hat)
  #0.01007748
```













