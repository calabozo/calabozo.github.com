---
layout: page
title: Testing your data
excerpt: "ANOVA, t-student, chi square"
search_omit: false
---

http://researchbasics.education.uconn.edu/anova_regression_and_chi-square/



# 1. Investigating differences

## 1.1. Confidence Intervals

We have an array of data, we want to calculate the mean and its interval confidence assuming a gaussian variable.

### Known variance

From the array of data we can calculate the mean $$E[X]$$. The variance is known and equal to $$\sigma^2$$, and the **standard error of the mean** is $$\sigma^2_M=\frac{\sigma^2}{n}$$ where n is the number of samples of our data.

The confidence interval for the mean is given by the following formula:  
\\[\text{Lower limit}=E[X]-Q(p)·\sigma_M=E[X]-Q(p)·\frac{\sigma}{\sqrt(n)} \\]
\\[\text{Upper limit}=E[X]+Q(p)·\sigma_M=E[X]+Q(p)·\frac{\sigma}{\sqrt(n)} \\]

The value of $$Q(p)$$ represents the value required by a [gaussian](https://en.wikipedia.org/wiki/Q-function) cumulative density function to obtain the probability p. 
For example if we want to calculate the margin 5% for a vector, we will have:
\\[p=1-\frac{0.5}{2}=0.975 \\]
\\[Q(0.975)=1.959964 \\]



In R this will look like this:
```R
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

From the array of data we can calculate the mean $$E[X]$$. The variance is unknown, it will be calculated as $$Var[X]$$ (this is the [unbiased estimator](https://en.wikipedia.org/wiki/Bias_of_an_estimator) of the variance) and the standard error of the mean is $$s^2_M=\frac{VAR[X]}{n}$$ where n is the number of samples of our data.

The confidence interval for the mean is given by the following formula:
\\[ \text{Lower limit}=E[X]-t(p,n-1)·s_M=E[X]-t(p,n-1)·\frac{\sqrt{Var[X]} }{\sqrt{n}} \\]
\\[ \text{Upper limit}=E[X]+t(p,n-1)·s_M=E[X]+t(p,n-1)·\frac{\sqrt{Var[X]} }{\sqrt{n}} \\]

The value of t(p,n-1) represents the value required by a [t-student](https://en.wikipedia.org/wiki/Student%27s_t-distribution) cumulative density function with $$n-1$$ degree of freedom to give a $p$ probability.
For example if we want to calculate the margin 5% for a vector of length 10, we will have:
\\[p=1-\frac{0.5}{2}=0.975\\]
\\[t(0.975,9)=1.959964\\]

In R this will look like this:

```R
> n=20
> X<-rnorm(n,mean=7,sd=3)
> m<-mean(X)
> sm=sd(X)/sqrt(n)
> error <- qt(0.975,n-1)*sm
> upl <- m+error
> lowl <- m-error
```


The mean is 7.283274 with an error of ±1.000087

For the same variance this confidence interval is larger than the one obtained with gaussian.

## 1.2. Comparing two groups of data

### Student's t-test

The [t-Test](https://en.wikipedia.org/wiki/Student's_t-test) is used when you have **two** groups of data and you want to know if the **mean** of both groups is the same or not. We have two hypotesis:
\\[H_0 : \mu(X) = \mu(Y) \\]
\\[H_1 : \mu(X) \ne \mu(Y) \\]

We start the same asumption:
1. The samples have been randomly selected from their populations.
2. The populations are normally distributed. (if not see [Wilcoxon-Mann-Whitney test](#wilcoxon-mann-whitney-test))
3. Both populations have the same variance. (if not see [Welch's t-test](#welchs-t-test))
4. The populations are not correlated. (if not see [Paired t-test](#paired-t-test))



We first calculate the mean and the variance of both groups. The t statistic to test whether the $$H_0$$ hypotesis is true can be calculated as follows:
\\[ t=\frac{E[X]-E[Y]}{s_p·\sqrt{\frac{1}{n_x}+\frac{1}{n_y}}} \\]

Where $$s_p$$ is the [pooled standard deviation](https://en.wikipedia.org/wiki/Pooled_variance), it is calculated as:

\\[ s^2_p=\frac{(n_x-1)Var[X]+(n_y-1)Var[Y]}{n_x+n_y-2} \\]

Where $$n_x$$ and $$n_y$$ are the number of samples in each of the sample groups. The number of degrees of freedom is: $$d.f.=n_x+n_y-2$$.
We can assume that the two variance are equal if they pass the [Fisher variance F-test](fisher-variance-f-test):

The code in R will be:
```R
> nx<-10
> ny<-15
> X<-rnorm(nx,mean=7,sd=3)
> Y<-rnorm(ny,mean=8,sd=3)
> sp<-sqrt(((nx-1)*var(X)+(ny-1)*var(Y))/(nx+ny-2))
> sp
[1] 3.215225
> t<-(mean(X)-mean(Y))/(sp*sqrt(1/nx+1/ny))
> t
> pt(t,nx+ny-2)*2
[1] 0.6262657
```
We multiply the original probability by 2 because we are assuming the two tailed hypothesis (we don't care which of both means is larger). 
This can be complete using the *t.test* command:
```R
> t.test(X,Y,var.equal=TRUE)

	Two Sample t-test

data:  X and Y
t = -0.49361, df = 23, p-value = 0.6263
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 -3.363252  2.067430
sample estimates:
mean of x mean of y 
 6.886126  7.534037 
```
So, in this case we accept the null hypothesis, both means are equal.


#### Welch's t-test ####

This test is used when the two population variances are not assumed to be equal. The t statistic is calculated as:
\\[ t=\frac{E[X]-E[Y]}{s_{\bar{\Delta}}} \\]

where
\\[ s_{\bar{\Delta}}=\sqrt{\frac{Var[X]}{n_x}+\frac{Var[Y]}{n_y}} \\]

And the number of degrees of freedom is:
\\[ d.f.= \frac{\left(\frac{Var[X]}{n_x}+\frac{Var[Y]}{n_y} \right)^2}{\frac{\left( \frac{Var[X]}{n_x} \right)^2}{n_x-1} + \frac{\left( \frac{Var[Y]}{n_y} \right)^2}{n_y-1}} \\]

```R
> X<-rnorm(nx,mean=7,sd=3)
> Y<-rnorm(ny,mean=9,sd=0.1)
> sd<-sqrt(var(X)/nx+var(Y)/ny)
> t<-(mean(X)-mean(Y))/(sd)
> t
[1] -3.65098
> df<-(var(X)/nx+var(Y)/ny)^2/((var(X)/nx)^2/(nx-1)+(var(Y)/ny)^2/(ny-1))
> df
[1] 9.031248
> pt(t,df)*2
[1] 0.005278576
```
This can be easily done with the R command *t.test*:
```R
> t.test(X,Y,var.equal=FALSE)

	Welch Two Sample t-test

data:  X and Y
t = -3.651, df = 9.0312, p-value = 0.005279
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 -4.718905 -1.109508
sample estimates:
mean of x mean of y 
 6.087772  9.001978 
```
In this case the p-value is really low so we can discard the null hypothesis and assume that both means are not equal.



#### Paired t-test ####

This test is used when there are **two** groups of data which are correlated and you want to know if the **mean** of both gorups is the same or not. This can be used for example if we have a medical study and we want to check is some values change after the threatment. The null hypoteis states that the difference between the means is equal to $$\mu_0$$:

\\[H_0 : \mu(X) - \mu(Y) = \mu_0 \\]
\\[H_1 : \mu(X) -\mu(Y) \ne \mu_0 \\]

The t statistic is calculated from the mean and variance of the difference of the two samples:
\\[ t=\frac{E[X-Y]-\mu_0}{\sqrt{var[X-Y]/n}}\\]

In R this can be calculated as:
```R
> n<-20
> X<-rnorm(n,mean=5,sd=4)
> Y<-X-rnorm(n,mean=3,sd=2)
> mu0<-3
> D<-X-Y
> t<-(mean(D)-mu0)/(sqrt(var(D)/nx))
> t
[1] -0.02351163
> pt(t,n-1)*2
[1] 0.9814873
```

or with the *t.test* function:
```R
> t.test(X,Y,mu=mu0,paired=TRUE)

	Paired t-test

data:  X and Y
t = -0.023512, df = 19, p-value = 0.9815
alternative hypothesis: true difference in means is not equal to 3
95 percent confidence interval:
 2.119336 3.861099
sample estimates:
mean of the differences 
               2.990217 

```






### Wilcoxon-Mann-Whitney test ####


A very general formulation extracted from [wikipedia](https://en.wikipedia.org/wiki/Mann%E2%80%93Whitney_U_test) is to assume that:

1. All the observations from both groups are independent of each other,
1. The responses are ordinal (i.e., one can at least say, of any two observations, which is the greater)
1. Under the null hypothesis $$H_0$$, the probability of an observation from the population X exceeding an observation from the second population Y equals the probability of an observation from Y exceeding an observation from X. A stronger null hypothesis commonly used is *The distributions of both populations are equal* which implies the previous hypothesis.
1. The alternative hypothesis $$H_1$$ is the probability of an observation from the population X exceeding an observation from the second population Y is different from the probability of an observation from Y exceeding an observation from X. The alternative may also be stated in terms of a one-sided test, for example: P(X > Y) > P(Y > X).

\\[H_0 : P(X > Y) = P(Y > X) \\]
\\[H_1 :P(X > Y) \ne P(Y > X)\\]

This can be calculated in R with the [wilcox.test](http://www.r-tutor.com/elementary-statistics/non-parametric-methods/mann-whitney-wilcoxon-test) function:
```
> mtcars$mpg
 [1] 21.0 21.0 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 17.8 16.4 17.3 15.2 10.4
[16] 10.4 14.7 32.4 30.4 33.9 21.5 15.5 15.2 13.3 19.2 27.3 26.0 30.4 15.8 19.7
[31] 15.0 21.4
> mtcars$am 
 [1] 1 1 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 0 0 0 0 0 1 1 1 1 1 1 1
> wilcox.test(mpg ~ am, data=mtcars) 

	Wilcoxon rank sum test with continuity correction

data:  mpg by am
W = 42, p-value = 0.001871
alternative hypothesis: true location shift is not equal to 0

Warning message:
In wilcox.test.default(x = c(21.4, 18.7, 18.1, 14.3, 24.4, 22.8,  :
  cannot compute exact p-value with ties
```
We can assume that the gas milegage data from the column *mtcars$mpg* for automated cars (*mtcars$am==1*) is independent to the one for non-automated cars (*mtcars$am==1*) because the low p-value leave us to reject the $$H_0$$ hypothesis.





### Fisher variance F-test ###

The Fisher [F-Test](https://en.wikipedia.org/wiki/F-test_of_equality_of_variances) is used when you have **two** groups of data and you want to know if the **variance** of both groups is the same or not. This means that the variables are [homoscedastic](https://en.wikipedia.org/wiki/Homoscedasticity). We have two hypotesis:
\\[H_0 : \sigma(X) = \sigma(Y) \\]
\\[H_1 : \sigma(X) \ne \sigma(Y) \\]

The test statistic is based on the cocient of the two variances:
\\[F=\frac{Var[X]}{Var[Y]}\\]

this follows an [F-distribution](https://en.wikipedia.org/wiki/F-distribution) with $$n_x-1$$ and $$n_y$$ degrees of freedom.

```R
> nx<-10
> ny<-15
> X<-rnorm(nx,mean=7,sd=3)
> Y<-rnorm(ny,mean=8,sd=3)
> f<-var(X)/var(Y)
> pf(f,nx,ny)
[1] 0.6579915
```

It is also possible to use the function var.test:
```R
> var.test(X,Y)
	F test to compare two variances

data:  X and Y
F = 1.2405, num df = 9, denom df = 14, p-value = 0.6924
alternative hypothesis: true ratio of variances is not equal to 1
95 percent confidence interval:
 0.386538 4.711423
sample estimates:
ratio of variances 
          1.240517 

```
Thus in this case with a p-value of 0.6924 we can accept the null hypotesis and assume that the two variances are homogeneous.


## 1.3. Comparing several groups of data

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


## 1.4. Frequency tables

### Chi-square

The Chi-square test can also be used to test for independence between two variables
* The null hypothesis for this test is that the variables are independent (i.e. that there is no statistical association).
* The alternative hypothesis is that there is a statistical relationship or association between the two variables.

The Chi-square test can be used to test for equality of proportions between two or more groups.
* The null hypothesis for this test is that the 2 proportions are equal.
* The alternative hypothesis is that the proportions are not equal (test for a difference in either direction)

Then Pearson's chi-squared test is performed
     of the null hypothesis that the joint distribution of the cell
     counts in a 2-dimensional contingency table is the product of the
     row and column marginals.

### Yates 


### Fisher

Fisher's exact test, as its name implies, always gives an exact P value and works fine with small sample sizes. Fisher's test (unlike chi-square) is very hard to calculate by hand, but is easy to compute with a computer. 

### Barnard 

An alternative to Fisher's test is the Barnard test. Fisher's test is said to be 'conditional' on the row and column totals, while Barnard's test is not. Mehta and Senchaudhuri explain the difference and why Barnard's test has more power (2). Berger modified this test to one that is easier to calculate yet more powerful. 


