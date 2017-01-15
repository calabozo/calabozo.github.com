---
layout: default
title: Graph theory
excerpt: 
search_omit: false
---

# Network Inference 

The purpose of this section is to explain how to obtain a network of related elements from different data. We can have for example gene expression data where each row represents the gene expression level per each condition. Or we can have a computer network and see if from the correlation we can infer the network structure.

## Basic network inference

We can calculate the correlation and obtain the relations of different elements.

Pearson correlation:
https://en.wikipedia.org/wiki/Pearson_correlation

Speakman's rank correlation coefficient:
The Spearman correlation coefficient is defined as the Pearson correlation coefficient between the ranked variables
https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient

Kendall rank correlation coefficient:
Commonly referred to as Kendall's tau coefficient (after the Greek letter τ), is a statistic used to measure the ordinal association between two measured quantities
https://en.wikipedia.org/wiki/Kendall_rank_correlation_coefficient


## Relevance network

Correlation can be misleading

```R
set.seed(2807)
x <- rnorm(100)
y <- 2*x+1+rnorm(100,0,0.1)

cor(x,y)
# [1] 0.9988261
z <- 2*x+1+rnorm(100,0,0.1)
cor(x,z)
# [1] 0.998751
cor(y,z)
# [1] 0.9971105
```

### Gaussian Graphical Model

In Gaussian Graphical Models, the conditional dependency graph is estimated. This graph is defined as follows 


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



