---
layout: default
title: Metaheuristic 
---

# Ensemble learning.

[Ensemble](https://en.wikipedia.org/wiki/Ensemble_learning) methods use multiple learning algorithms to obtain better predictive performance than could be obtained from any of the constituent learning algorithms alone.

# Bootstrap aggregating

Bootstrap aggregating, also called [bagging](https://en.wikipedia.org/wiki/Bootstrap_aggregating), is a machine learning ensemble meta-algorithm designed to improve the stability and accuracy of machine learning algorithms. It also reduces variance and helps to avoid overfitting.
Given a standard training set D of size n, bagging generates m new training sets $$ D_{i}$$, each of size n', by sampling from D uniformly and with replacement. By sampling with replacement, some observations may be repeated in each $$D_i$$. The m models are fitted using the above m bootstrap samples and combined by averaging the output (for regression) or voting (for classification).

# Boosting

[Boosting](https://en.wikipedia.org/wiki/Boosting_(machine_learning)) involves incrementally building an ensemble by training each new model instance to emphasize the training instances that previous models mis-classified. In some cases, boosting has been shown to yield better accuracy than bagging, but it also tends to be more likely to over-fit the training data. This algorithm familiy rely on training several *weak lerners* which are algorithms that have a poor response (but better than purely random). The output of this *weak learners* is combined with different weights dependeing on the *weak learners* accuracy.

## Adaboost

This meta-algorithm is used when we want to improve the quality of a learning algorithm. We combine the output of different weight learners into a weighted sum that represents the final ouput of the boosted algorithm.

