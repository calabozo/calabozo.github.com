---
layout: page
title: Testing your data
excerpt: "ANOVA, t-student, chi square"
search_omit: false
---

1. 
{:toc}



# Confidence Intervals

We have an array of data, we want to calculate the mean and its interval confidence assuming a gaussian variable.

## Known variance

From the array of data we can calculate the mean $$E[X]$$. The variance is known and equal to $$\sigma^2$$, and the **standard error of the mean** is $$\sigma^2_M=\frac{\sigma^2}{n}$$ where n is the number of samples of our data.

The confidence interval for the mean is given by the following formula:  
\\[\text{Lower limit}=E[X]-Q(p)·\sigma_M=E[X]-Q(p)·\frac{\sigma}{\sqrt(n)} \\]
\\[\text{Upper limit}=E[X]+Q(p)·\sigma_M=E[X]+Q(p)·\frac{\sigma}{\sqrt(n)} \\]

The value of $$Q(p)$$ represents the value required by a [gaussian](https://en.wikipedia.org/wiki/Q-function) cumulative density function to obtain the probability p. 
For example if we want to calculate the margin 5% for a vector, we will have:
\\[p=1-\frac{0.05}{2}=0.975 \\]
\\[Q(0.975)=1.959964 \\]



In R this will look like this:
```R
> n=20
> s=3
> X<-rnorm(n,mean=7,sd=s)
> m<-mean(X)
> sm=s/sqrt(n)
> error <- qnorm(0.975)*sm
> upl <- m+error
> lowl <- m-error
> print(paste0("The mean is ",m," with an error of ±",error))
[1] "The mean is 6.05145283234904 with an error of ±1.31478381086487"
```


## Unknown variance

From the array of data we can calculate the mean $$E[X]$$. The variance is unknown, it will be calculated as $$Var[X]$$ (this is the [unbiased estimator](https://en.wikipedia.org/wiki/Bias_of_an_estimator) of the variance) and the standard error of the mean is $$s^2_M=\frac{VAR[X]}{n}$$ where n is the number of samples of our data.

The confidence interval for the mean is given by the following formula:
\\[ \text{Lower limit}=E[X]-t(p,n-1)·s_M=E[X]-t(p,n-1)·\frac{\sqrt{Var[X]} }{\sqrt{n}} \\]
\\[ \text{Upper limit}=E[X]+t(p,n-1)·s_M=E[X]+t(p,n-1)·\frac{\sqrt{Var[X]} }{\sqrt{n}} \\]

The value of t(p,n-1) represents the value required by a [t-student](https://en.wikipedia.org/wiki/Student%27s_t-distribution) cumulative density function with $$n-1$$ degree of freedom to give a $$p$$ probability.
For example if we want to calculate the margin 5% for a vector of length 10, we will have:
\\[p=1-\frac{0.05}{2}=0.975\\]
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
> print(paste0("The mean is ",m," with an error of ±",error))
[1] "The mean is 6.55115534470227 with an error of ±0.959080458598336"
```
For the same samples **this confidence interval is larger than the one obtained with gaussian**.


# Comparing two groups quantitative of data

## Student's t-test

The [t-Test](https://en.wikipedia.org/wiki/Student's_t-test) is used when you have two groups of data and you want to know if the **mean of both groups is the same or not**. We have two hypotesis:
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
We can assume that the two variances are equal if they pass the [Fisher variance F-test](#fisher-variance-f-test):

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
[1] -0.49361
> pvalue<-pt(t,nx+ny-2)*2
> print(paste("The pvalue is",pvalue))
[1] "The pvalue is 0.6262657"
```
We multiply the original probability by 2 because we are assuming the two tailed hypothesis (we don't care which of both means is larger). 
The pvalue is aproximately 0.63 which means that there is a 63% possibility that the means of X and Y are equal. So, in this case we cannot reject the null hypothesis.


The same procedure can be easily complete using the *t.test* command:
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



### Welch's t-test

This test is used when the two population **variances are not assumed to be equal**. The t-statistic is calculated as:
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
In this case the p-value is really low so we can reject the null hypothesis and assume that both means are not equal.



### Paired t-test ####

This test is used when there are **two** groups of data **which are correlated** and you want to know if the **mean** of both groups is the same or not. This can be used for example if we have a medical study and we want to check if some values change after the treatment. The null hypothesis states that the difference between the means is equal to $$\mu_0$$:

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



## Wilcoxon-Mann-Whitney test ####


The [Mann–Whitney U test](https://en.wikipedia.org/wiki/Mann%E2%80%93Whitney_U_test) is used for **compare if two independent samples come from the same distribution**. A very general formulation is to assume that:

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
We can assume that the gas mileage data from the column *mtcars$mpg* for automated cars (*mtcars$am==1*) is independent to the one for non-automated cars (*mtcars$am==1*) because the low p-value leave us to reject the $$H_0$$ hypothesis.





## Fisher variance F-test

The Fisher [F-Test](https://en.wikipedia.org/wiki/F-test_of_equality_of_variances) is used when you have **two** groups of data and you want to know if the **variance** of both groups is the same or not. This means that the variables are [homoscedastic](https://en.wikipedia.org/wiki/Homoscedasticity). We have two hypotesis:
\\[H_0 : \sigma(X) = \sigma(Y) \\]
\\[H_1 : \sigma(X) \ne \sigma(Y) \\]

The test statistic is based on the quotient of the two variances:
\\[F=\frac{Var[X]}{Var[Y]}\\]

this follows an [F-distribution](https://en.wikipedia.org/wiki/F-distribution) with $$n_x-1$$ and $$n_y-1$$ degrees of freedom.

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


# Comparing several groups of data

## one-way ANOVA

We use [one-way ANOVA](https://en.wikipedia.org/wiki/One-way_analysis_of_variance) to **compare the means of three or more groups of data**. It is also said that compares one dependent variable (the input data) with an independent variable that has more than two levels; each level, also called factor will identify each of the different groups of data. The *null hypothesis* assumes that all the groups have the same mean values:
\\[H_0 : \mu_1 = \mu_2 = ... = \mu_k \\]
\\[H_1 : \text{at leas one pair }\mu_j\ne \mu_i \\]
These hypothesis examine the model fit the model: $$y_{ij}=\mu_j+\varepsilon_{ij}$$, where:
* $$y_{ij}$$ is the dependent variable, i sample from group j
* $$\mu_j$$ is the group j real mean
* $$\varepsilon_{ij}$$ are the errors results of the model
* $$\bar{y}_j$$ is the group j sample mean
* $$k$$ is the number of groups
* $$n_j$$ is the sample size of group j
* $$n=\sum_{j=1}^{k}n_j$$ is the total sample size

The F statistics of the [omnibus test](https://en.wikipedia.org/wiki/Omnibus_test#Omnibus_Tests_in_One_Way_Analysis_of_Variance) in one-way anova:
\\[
F=\frac{\sum_{j=1}^{k} n_j \left ( \bar{y_j}-\bar{y} \right )^2 / \left ( k-1 \right ) }{\sum_{j=1}^{k} \sum_{i=1}^{n_j} \left ( y_{ij}-\bar{y_j} \right )^2 / \left ( n-k \right ) }
\\]


The ANOVA produces an [F-statistic](# Fisher variance F-test), the ratio of the variance calculated among the means to the variance within the samples. *If the group means are drawn from populations with the same mean values, the variance between the group means should be lower than the variance of the samples*, following the central limit theorem. A higher ratio therefore implies that the samples were drawn from populations with different mean values.

When there are only two means to compare the [Student's t-test](# Student's t-test) should be used, the t-test and the F-test are equivalent; the relation between ANOVA and t is given by $$F= t^2$$. 


ANOVA assumptions:
* The dependent variable is normally distributed in each group. (if not use [Kruskal-Wallis H Test](# Kruskal-Wallis H Test))
* There is homogeneity of variances. (if not use [Welch F test](# Welch F test))
* Independence of observations. 


We can calculate in R the one-way ANOVA for 3 gaussian random variables:
```R
> n1<-40; n2<-50; n3<-70
> m1<-18; m2<-15; m3<-15
> sd<-7
> 
> a1<-rnorm(n1,mean=m1,sd=sd)
> a2<-rnorm(n2,mean=m2,sd=sd)
> a3<-rnorm(n3,mean=m3,sd=sd)
> 
> ma1<-mean(a1)
> ma2<-mean(a2)
> ma3<-mean(a3)
> ma<-mean(c(a1,a2,a3))
> 
> masq_between_group<-n1*(ma1-ma)^2+n2*(ma2-ma)^2+n3*(ma3-ma)^2
> df_between_group<-3-1
> mean_square_value_between_group<-masq_between_group/df_between_group
[1] 905
> 
> masq_within_group<-sum((a1-ma1)^2)+sum((a2-ma2)^2)+sum((a3-ma3)^2)
> df_within_group<-n1-1+n2-1+n3-1
> mean_square_value_within_group<-masq_within_group/df_within_group
[1] 6845
> 
> F<-mean_square_value_between_group/mean_square_value_within_group
> F
[1] 10.38407
> pvalue<-1-pf(F,df_between_group,df_within_group)
> pvalue
[1] 5.815527e-05
> 
```
The p-value is really low, so we can reject the null hypotesis. This means that not all the groups have the same mean.

The same procedure can be easily done with the build-in R function *aov*:
```R
> df <- rbind(data.frame(value=a1,cl="1"),data.frame(value=a2,cl="2"),data.frame(value=a3,cl="3"))
> summary(aov( value ~ cl, data=df))
             Df Sum Sq Mean Sq F value   Pr(>F)    
cl            2    905   452.7   10.38 5.82e-05 ***
Residuals   157   6845    43.6                     
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

### Turkey's test

[Tukey's range test](https://en.wikipedia.org/wiki/Tukey%27s_range_test), also known as Tukey's HSD (honest significant difference) test, can be used to **find the means that are significant different from each of the groups**. Tukey's test compares the means of every group to the means of every other group and identify any difference between two means greater than the expected standard error.
It is equivalent to a pairwise t-student test with a small but important difference. When doing Turkeys test homogeneity is assumed, so the variance is calculated with all the samples from all the groups. When doing pairwise t-student the variance for each group is calculated independently which will lead to a less robust result.

We can calculate the Tukey test in R with the data from the last example:
```
> TukeyHSD(aov( value ~ cl, data=df))
  Tukey multiple comparisons of means
    95% family-wise confidence level

Fit: aov(formula = value ~ cl, data = df)

$cl
          diff       lwr       upr     p adj
2-1 -5.8671081 -9.181346 -2.552870 0.0001375
3-1 -5.1484894 -8.245158 -2.051821 0.0003666
3-2  0.7186187 -2.174285  3.611523 0.8268771

```
This means that group 3 and 2 have a high probability to have the same mean, and the mean of group 1 differs.

## Welch's anova

Does not require the homogeneity of variances assumption. It is based on the [Welch's t-test](# Welch's t-test).

In R it can be invoked using the command *oneway.test* with the parameter *var.equal=FALSE*. The syntax is similar to *aov*.


### Kruskal-Wallis H Test 

The Kruskal–Wallis test by ranks, Kruskal–Wallis H test, or One-way ANOVA on ranks is a **non-parametric method for testing whether samples originate from the same distribution**. It is used for comparing two or more independent samples of equal or different sample sizes. It extends the [Wilcoxon-Mann-Whitney test](# Wilcoxon-Mann-Whitney test).

This can be implemented in R with the command *kruskal.test*. An example of how to use it can be found at [r-tutor](http://www.r-tutor.com/elementary-statistics/non-parametric-methods/kruskal-wallis-test).



## two-way ANOVA

The [two-way ANOVA](https://en.wikipedia.org/wiki/Two-way_analysis_of_variance) is an extension of the [one-way ANOVA](#one-way ANOVA) where there are two variables (factors) which interact with the data. 
In this case the data is given in a matrix form where cell will have the samples corresponding to the two dependent factors, one given by the row number and other for the column number. 


These hypothesis try to fit the data to the following model: $$y_{i}=\beta_0+\beta_1x_{i1}+\beta_2x_{i2}+\beta_3x_{i1}x_{i2}+\varepsilon_{i}=\widehat{y_i}+\varepsilon_{i}$$

where:
* $$y_{i}$$ is the dependent variable
* $$\beta_0$$ is an unknown coefficient to be estimated independent of the other variables.
* $$\beta_1$$ is an unknown coefficient to be estimated for the first independent variable.
* $$\beta_2$$ is an unknown coefficient to be estimated for the second independent variable.
* $$\beta_3$$ is an unknown coefficient to be estimated for the iteration between the two dependent variables.
* $$x_{i1}$$ is first dependent variable associated with the i-th sample
* $$x_{i2}$$ is second dependent variable associated with the i-th sample
* $$\varepsilon_{i}$$ are the errors results of the model
* $$\widehat{y_i}$$ is the i-th sample estimated value.
* $$k$$ is the number of unknown coefficients, for two-way anova is 3.
* $$n=$$ is the total sample size

The F statistics of the [omnibus test](https://en.wikipedia.org/wiki/Omnibus_test#Omnibus_Tests_in_Multiple_Regression) in two-way anova:
\\[
F=\frac{\sum_{j=1}^{n} \left ( \widehat{y_j}-\bar{y} \right )^2 / k }{\sum_{j=1}^{k} \sum_{i=1}^{n_j} \left ( y_{ij}-\widehat{y_j} \right )^2 / \left ( n-k-1 \right ) }
\\]


The *null hypothesis* assumes that all the groups have the same mean values:
\\[H_0 : \beta_1 = \beta_2 = \beta_3 = 0\\]
\\[H_1 : \text{at leas one pair }\beta_j\ne \beta_i \\]

The asumptions are the same that we can find in the one-way ANOVA analysis:

```R
> varId1<-c(1,2)
> varId2<-seq(0,5,length=4)
> var1<-c(rep(varId1,each=50))
> var2<-c(rep(varId2,length.out=length(var1)))
> 
> y<-rnorm(length(var1),mean=0,sd=2)+var1+var2
> dfy<-data.frame(y,var1,var2)
> 
> summary(aov( y ~ var1*var2, data=dfy))
            Df Sum Sq Mean Sq F value   Pr(>F)    
var1         1   43.7   43.68   9.338  0.00291 ** 
var2         1  258.2  258.15  55.183 4.51e-11 ***
var1:var2    1    0.4    0.36   0.076  0.78300    
Residuals   96  449.1    4.68               
```

The summary shows that there is a dependency between the data and *var1* and *var2*, but there is no correlation between *var1* and *var2*.


## MANOVA

In statistics, multivariate analysis of variance ([MANOVA](https://en.wikipedia.org/wiki/Multivariate_analysis_of_variance)) is a procedure for comparing multivariate sample means. As a multivariate procedure, it is used when there are two or more independent variables, and is typically followed by significance tests involving individual independent variables separately.
It is a generalization of the [two-way anova](# two-way ANOVA).




# Contingency tables

## Pearson's Chi-square

The [Pearson's chi-square](https://en.wikipedia.org/wiki/Pearson%27s_chi-squared_test) test can be used to **test for independence between categorical data**.
* The *null hypothesis* for this test is that the variables are independent (i.e. that there is no statistical association).
* The alternative hypothesis is that there is a statistical relationship or association between the two variables.

These are the assumptions for this test:
* Simple random sample
* A sample with a sufficiently large size is assumed
* Expected categories count:  When this assumption is not met, [Yates's correction](https://en.wikipedia.org/wiki/Yates%27s_correction_for_continuity) is applied.
* Independence: chi-squared cannot be used to test correlated data (like matched pairs or panel data). In those cases you should use [McNemar's test](https://en.wikipedia.org/wiki/McNemar%27s_test)


The value of the test-statistic is:
\\[
\chi^2 = \sum_{i=1}^{n} \frac{(O_i-E_i)^2}{E_i} = N \sum_{i=1}^{n} \frac{(O_i/N-p_i)^2}{p_i} 
\\]

where:
* $$N$$ is the total number of observed elements.
* $$O_i$$ is the observed number elements in the category *i*.
* $$E_i$$ is the expected number elements in the category *i*.
* $$p_i$$ is the expected probability of finding an element in the category *i*. $$p_i=E_i/N$$  
* $$n$$ is the number of categories under study.

This chi-squared statistic is used to calculate a p-value by comparing the value of the statistic to a chi-squared distribution. 
For the case of a [multinomial goodness fit](https://en.wikipedia.org/wiki/Multinomial_test) the number of degrees of freedom is $$n-1$$.

Let's imagine that we want to check if a 4-sided dice. We roll it 40 times and the observed number of times each side appears is 10,7,9,14:
```R
> observed<-c(10,7,9,14)
> N<-sum(observed)
> expected<-N/length(observed)
> test<-sum((observed-expected)^2/expected)
> 
> pvalue<-1-pchisq(test,length(observed)-1)
> pvalue
[1] 0.4574895
```
The p-value to accept null hypotesis is really high, so we can discard $$H_0$$ and assume that the dice is not fair. The function *chisq.test* from R can be used for this purpose.


In this case, an "observation" consists of the values of two outcomes and the null hypothesis is that the occurrence of these outcomes is statistically independent. Each observation is allocated to one cell of a two-dimensional array of cells (called a contingency table) according to the values of the two outcomes. If there are r rows and c columns in the table, the *theoretical frequency* for a cell, given the hypothesis of independence, is
\\[
    E_{i,j}= N p_{i·} p_{·j}
\\]
In this case the value of the test-statistic is:
\\[
\chi^2 = \sum_{i=1}^{r}\sum_{i=1}^{c} \frac{(O_{i,j}-E_{i,j})^2}{E_{i,j}} = N \sum_{i=1}^{r}\sum_{i=1}^{c} \frac{(O_{i,j}/N-p_{i·}p_{·j})^2}{p_{i·}p_{·j}} 
\\]
In this case the number of degrees of freedom is $$(r-1)(c-1)$$.



## Fisher

[Fisher's exact test](https://en.wikipedia.org/wiki/Fisher%27s_exact_test), as its name implies, always gives an exact P value and works fine with small sample sizes. Fisher's test (unlike chi-square) is very hard to calculate by hand, but is easy to compute with a computer. 

It can be done in R with the [fisher.test](https://www.r-bloggers.com/contingency-tables-%E2%80%93-fisher%E2%80%99s-exact-test/) function.


## Barnard

An alternative to Fisher's test is the [Barnard test](https://en.wikipedia.org/wiki/Barnard%27s_test). Fisher's test is said to be 'conditional' on the row and column totals, while Barnard's test is not. It examines the association of two categorical variables and is a more powerful alternative than Fisher's exact test for 2×2 contingency tables.

It can be done in R with the [Barnard](https://cran.r-project.org/web/packages/Barnard/) package.


# Survival distributions


## Log-rank test

In statistics, the [log-rank](https://en.wikipedia.org/wiki/Log-rank_test) test is a hypothesis test to compare the survival distributions of two samples. It is a nonparametric test and appropriate to use when the data are right skewed and censored (technically, the censoring must be non-informative).

Tests if there is a difference between two or more survival curves. More information about survival analysis can be found [here](/statistics/survival/).


In the package *survival* the dataset *colon* contains data from one of the first successful trials of adjuvant chemotherapy for colon cancer. We want to know if the *sex* has any impact on the survival rate. The columns of interest in this case are:
* time:	 days until event or censoring
* sex: 1=male
* status:	 censoring status
```R
> library(survival)
> survdiff(Surv(time, status) ~ sex,data=colon, rho=0)
Call:
survdiff(formula = Surv(time, status) ~ sex, data = colon, rho = 0)

        N Observed Expected (O-E)^2/E (O-E)^2/V
sex=0 890      444      436     0.136     0.259
sex=1 968      476      484     0.123     0.259

 Chisq= 0.3  on 1 degrees of freedom, p= 0.611 
```
This means that we cannot assume that the *sex* is independent from the distribution.

