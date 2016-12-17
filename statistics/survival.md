---
layout: default
title: Survival analysis
excerpt: ""
search_omit: false
---

## Definitions

**Survival Function**  
It is the survival probability given a point of time, so it is a function monotically decreasing in the range [0,1]. It can be estimated with the [Kaplan Meier](https://en.wikipedia.org/wiki/Kaplan%E2%80%93Meier_estimator) curve:

\\[\hat{S}(t) = \prod_{t_i \leq t}\frac{n_i-d_i}{n_i} \\]

Where $$n_j$$ is the original population at risk at time $$t_j$$, and $$d_j$$ is the number of events that happen to that population at $$t_j$$. The variance of this estimator can be calculated with the *Greenwood's formula*:

\\[Var[\hat{S}(t)]=\hat{S}(t)^2\sum_{t_i \leq t}\frac{d_i}{n_i·(n_i-d_i)} \\]


There is a matrix version of the Kaplan Meier estimator called the [Aalen-Johansens estimator](https://cran.r-project.org/web/packages/survival/vignettes/compete.pdf). It can be used to estimate the tranition probability matrix of a Markov process with a finite number of states. 




**Density Function**  
It is defined as the minus derivative of the *survival function*. The probability of happening event at time t, it is an *unconditional probability*: 
\\[f(t)=-\frac{\delta}{\delta t}S(t),\; \;   t>0 \\]

**Hazard function**  
Also known as the *incidence function*, measures the risk of having an event in a short interval $$[t,t+\Delta t]$$ inmediately after $$t$$ given that is has survived up to $$t$$. Given that there has no be an event until time $$t$$, it measures the probability of an event at time t. It is conventionally denoted by $$\lambda(t)$$ or $$h(t)$$:
\\[\lambda(t)=h(t)=\lim_{\Delta t\rightarrow 0}\frac{P(t \leq T <t+\Delta t) | T \geq t)}{\Delta t}=
\lim_{\Delta t\rightarrow 0}\frac{P(t \leq T <t+\Delta t)}{\Delta t·S(t)}=\frac{f(t)}{S(t)}\\]

In the continuous real this function can have any value greater than 0, but in the discrete domain its value is between 0 and 1. It is easy to demonstrate that:
\\[f(t)=S(t)\lambda(t)\\]
which means that to experience an event at $$t$$ you first have to survive to that time. This is a difference with the density function which is an *unconditional* probabilty while the hazard function is *conditional*.


**Cumulative hazard function**  
It is the integral of the *hazard function*:  
\\[H(t)=\int_{0}^{t}{h(s)ds}\;,\;t\geq 0 \\]

The non-parametric estimator [Nelson–Aalen](https://en.wikipedia.org/wiki/Nelson%E2%80%93Aalen_estimator) can approach this function by:
\\[\hat{H}(t)=\sum_{t_j \leq t}\frac{d_i}{n_i}\;,\;t\geq 0 \\]  
with $$d_{i}$$ the number of events at $$t_{i}$$ and $$n_{i}$$ the total individuals at risk at time $$t_{i}$$.


**Cumulative incidence**  
Probability that a particular event has ocured before a given time.
\\[CI(t)= \int_{0}^{t}{f(s)ds} = \int_{0}^{t}{S(s)\lambda(s)ds} \;,\;t\geq 0 \\]

