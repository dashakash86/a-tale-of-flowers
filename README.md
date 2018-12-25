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









