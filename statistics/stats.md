---
layout: page
title: Testing your data
excerpt: "ANOVA, t-student, chi square"
search_omit: false
---
http://researchbasics.education.uconn.edu/anova_regression_and_chi-square/

# Investigating differences

## Confidence Interval

We have an array of data, we want to calculate the mean and its interval confidence assuming a gaussian variable.

### Known variance

From the array of data we can calculate the mean $$E[X]$$. The variance is known and equal to $$\sigma^2$$, and the standard error of the mean is $$\sigma^2_M=\frac{sigma^2}{n}$$ where n is the number of samples of our data.

The confidence interval for the mean is given by the following formula:  
$$\text{Lower limit}=E[X]-Q(p)·\sigma_M 
\text{Upper limit}=E[X]+Q{p)·\sigma_M$$

The value of Q{p} represents the value required by a [gaussian](https://en.wikipedia.org/wiki/Q-function) cumulative density function. 
For example if we want to calculate the margin 5% for a vector, we will have:
\\[
p=1-\frac{0.5}{2}=0.975
Q(0.975)=1.959964
\\]

In R this will look like this:
```
 n=20
 s=3
 X<-rnorm(n,mean=7,sd=s)
 m<-mean(X)
 sm=s/sqrt(n)
 error <- qnorm(0.975)*sm
 upl <- m+error
 lowl <- m-error
```
The mean is 7.004721 with an error of ±1.314784.


### Unknown variance

From the array of data we can calculate the mean $$E[X]$$. The variance is unknown, it will be calculated as $$Var[X]$$ and the standard error of the mean is $$\sigma^2_M=\frac{sigma^2}{n}$$ where n is the number of samples of our data.

The confidence interval for the mean is given by the following formula:
\\[
\text{Lower limit}=E[X]-t(p,n-1)·Var[X]
\text{Upper limit}=E[X]+t(p,n-1)·Var[X]
\\]

The value of t{p} represents the value required by a [t-student](https://en.wikipedia.org/wiki/Student%27s_t-distribution) cumulative density function with $$n-1$$ degree of freedom.
For example if we want to calculate the margin 5% for a vector of length 10, we will have:
\\[
p=1-\frac{0.5}{2}=0.975
t(0.975,9)=1.959964
\\]

In R this will look like this:
'''
 n=20
 X<-rnorm(n,mean=7,sd=3)
 m<-mean(X)
 sm=sd(X)/sqrt(n)
 error <- qt(0.975,n-1)*sm
 upl <- m+error
 lowl <- m-error
'''
The mean is 7.283274 with an error of ±1.000087

For the same varianza this confidence interval is larger than the one obtained with gaussian.



## One Hyptesis with One independent Variable

You have **two** groups of data and you want to know if the mean of both groups is the same or not. We have two hipotesis:

### ANOVA

ANOVA assumptions
The dependent variable is normally distributed in each group that is being compared in the one-way ANOVA
There is homogeneity of variances. This means that the population variances in each group are equal.
Independence of observations. 


* the expected values of the errors are zero
* the variances of all errors are equal to each other
* the errors are independent
* they are normally distributed


```
> df <- rbind(data.frame(value=rnorm(100,mean=0,sd=0.5),cl="1"),data.frame(value=rnorm(100,mean=0,sd=0.5),cl="2"),data.frame(value=rnorm(100,mean=0,sd=0.5),cl="3"))
> summary(aov( value ~ cl, data=df))
             Df Sum Sq Mean Sq F value Pr(>F)
cl            2   0.41  0.2073   0.891  0.411
Residuals   297  69.13  0.2328               
> TukeyHSD(aov( value ~ cl, data=df))
  Tukey multiple comparisons of means
    95% family-wise confidence level

Fit: aov(formula = value ~ cl, data = df)

$cl
          diff         lwr       upr     p adj
2-1 0.01312482 -0.14759543 0.1738451 0.9798095
3-1 0.08460329 -0.07611697 0.2453235 0.4306080
3-2 0.07147847 -0.08924179 0.2321987 0.5474717

```

```
> df <- rbind(data.frame(value=rnorm(100,mean=0,sd=0.5),cl="1"),data.frame(value=rnorm(100,mean=0,sd=0.5),cl="2"),data.frame(value=rnorm(100,mean=5,sd=0.5),cl="3"))
> summary(aov( value ~ cl, data=df))
             Df Sum Sq Mean Sq F value Pr(>F)    
cl            2 1658.8   829.4    2891 <2e-16 ***
Residuals   297   85.2     0.3                   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
> TukeyHSD(aov( value ~ cl, data=df))
  Tukey multiple comparisons of means
    95% family-wise confidence level

Fit: aov(formula = value ~ cl, data = df)

$cl
          diff         lwr       upr     p adj
2-1 0.08096054 -0.09745352 0.2593746 0.5341514
3-1 5.02822427  4.84981021 5.2066383 0.0000000
3-2 4.94726373  4.76884967 5.1256778 0.0000000

```

### Welch F test

Does not require the homogeneity of variances asumption

###  Kruskal-Wallis H Test 

Does not require Gaussian the assumption.



