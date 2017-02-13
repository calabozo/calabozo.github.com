---
layout: default
title: Graph theory
excerpt: 
search_omit: false
---

# Network Inference 

The purpose of this section is to explain how to obtain a network of related elements from different data. We can have for example gene expression data where each row represents the gene expression level per each condition. Or we can have a computer network and see if from the correlation we can infer the network structure.

## Basic network inference

We can calculate the correlation and obtain the relations of different elements. The most popular is the [Pearson correlation](https://en.wikipedia.org/wiki/Pearson_correlation). It can be derived from the covariance matrix:
\\[\rho_{X,Y} = \frac{cov(X,Y)}{\sigma_X\sigma_Y} \\]

If we have an ordinal associaton, this means that instead of having the real numbers we have a sortered ranking with labels for the "first", "second", "third", etc... then we must use a [rank correlation](https://en.wikipedia.org/wiki/Rank_correlation). From all the available correlation coefficients, the most popular is the Speakman's rank correlation coefficient.
The [Spearman correlation coefficient](https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient) is defined as the Pearson correlation coefficient between the ranked variables. The Kendall rank correlation coefficient is also quite popular. Commonly referred to as [Kendall's tau coefficient](https://en.wikipedia.org/wiki/Kendall_rank_correlation_coefficient) (after the Greek letter τ), is a statistic used to measure the ordinal association between two measured quantities



## Relevance network

Correlation can be misleading, let's imagine that we have two functions:
\\[y=2·x+1+\varepsilon_0\\]
\\[z=2·x+1+\varepsilon_1\\]
where $$\varepsilon_0$$ and $$\varepsilon_1$$ are additive white random noise. This means that the variable x is highly correlated with y and z. But there is also a high correlation between z and y. Obviously we are interested in knowing if the relation between y and z is a true relation or they are correlated with a third variable (the variable x in this case).

```R
> set.seed(2807)
> x <- rnorm(100)
> y <- 2*x+1+rnorm(100,0,0.1)

> cor(x,y)
# [1] 0.9988261
> z <- 2*x+1+rnorm(100,0,0.1)
> cor(x,z)
# [1] 0.998751
> cor(y,z)
# [1] 0.9971105
```
Try to find this relations is very common in bioinformatics where relations between different genes are found looking at the evolution of the gene expression level and how they are related. This underlying network between different genes is what has been known as [Relevance network](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2232846/pdf/procamiasymp00004-0748.pdf)

### Partial correlation

The [partial correlation](https://en.wikipedia.org/wiki/Partial_correlation) measures the degree of association between two random variables. It is the correlation that remains between two variables if the effect of the other variables has been regressed away.


Formally, the partial correlation between $$y$$ and $$z$$ given a set of n controlling variables X = {X1, X2, ..., Xn}, written $$\rho_{zy·x}$$, is the correlation between the residuals $$R_z$$ and $$R_y$$ resulting from the linear regression of Z with X and of Y with X, respectively. In the [linear regression](/ml/linear_regression/) problem we are trying to obtain the optimum value of $$\beta$$ that minimizes the minimum quadratic error.
\\[y=\alpha+\beta·x+\epsilon\\]
This gives a $$\beta$$ of:
\\[\beta=\frac{\sum_{i=1}^n(y_i-\bar{y})(x_i-\bar{x})}{\sum_{i=1}^n(x_i-\bar{x})^2}=\frac{Cov(x,y)}{Var(x)}=X^t·X^{-1}·X^t·y\\]

The first-order partial correlation (i.e. when n=1) is the difference between a correlation and the product of the removable correlations divided by the product of the coefficients of alienation of the removable correlations. 


In Gaussian Graphical Models, the conditional dependency graph is estimated. This graph is defined as follows 

### Gaussian Graphical Model

#### LASSO

#### CLIME

#### TIGER

#### Greedy



### Bayesian networks

Are be able to deal with directed graphs and more precisely,  with  Directed  Acyclic  Graphs. R package bnlearn.

### Mutual information
[Meyer et al., 2008]  Meyer, P., Lafitte, F., and Bontempi, G. (2008). minet : A R/Bioconductor package for inferring large transcriptional networks using mutual information. BMC Bioinformatics , 9(461)



### Random Forest
[Huynh-Thu et al., 2010]  Huynh-Thu,  V.,  Irrthum,  A.,  Wehenkel,  L.,  and Geurts, P. (2010). Inferring regulatory networks from expression data using  tree-based methods. PLoS ONE, 5(9) :e12776.


### sparse CCA and sparse PLS
[Gonzàles et al., 2012]  Gonzàles,  I.,  Lê  Cao,  K.,  Davis,  M.,  and  Déjean,  S. (2012).  Visualising associations between paired ’omics data sets. BioData Mining , 5 :19.

R Package mixOmics





### Dependency network

https://en.wikipedia.org/wiki/Dependency_network


# Plot graph

## Minimum Spanning Tree 

A minimum spanning tree (MST) or minimum weight spanning tree is a subset of the edges of a connected, edge-weighted undirected graph that connects all the vertices together, without any cycles and with the minimum possible total edge weight. That is, it is a spanning tree whose sum of edge weights is as small as possible. More generally, any undirected graph (not necessarily connected) has a minimum spanning forest, which is a union of the minimum spanning trees for its connected components.

### Prim's algorithm

Prim's algorithm, which was invented by Jarnik in 1930 and rediscovered by Prim in 1957 and Dijkstra in 1959. Basically, it grows the MST (T) one edge at a time. Initially, T contains an arbitrary vertex. In each step, T is augmented with a least-weight edge (x,y) such that x is in T and y is not yet in T. By the Cut property, all edges added to T are in the MST. Its run-time is either O(m log n) or O(m + n log n), depending on the data-structures used.

### Kruskal's algorithm

Kruskal's algorithm is a minimum-spanning-tree algorithm which finds an edge of the least possible weight that connects any two trees in the forest.[1] It is a greedy algorithm in graph theory as it finds a minimum spanning tree for a connected weighted graph adding increasing cost arcs at each step.[1] This means it finds a subset of the edges that forms a tree that includes every vertex, where the total weight of all the edges in the tree is minimized. If the graph is not connected, then it finds a minimum spanning forest (a minimum spanning tree for each connected component).

# Planar Maximally Filtered Graph



