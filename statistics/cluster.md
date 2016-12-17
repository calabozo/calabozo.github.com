---
layout: page
title: Clustering
excerpt: 
search_omit: false
---

# Clustering algorithms

## K-means

[K-means](https://en.wikipedia.org/wiki/K-means_clustering) is the most popular and basic clustering algorithm. It is based centroids which are iteratively calculated in order to minimize the distance between the centroid and the elements which belong to that centroid.

Input parameters:
* k - number of clusters.

## K-medoids

[K-medoids](https://en.wikipedia.org/wiki/K-medoids) is closely related to k-means but instead of centroids it uses [metoids](https://en.wikipedia.org/wiki/Medoid) which are points from the dataset.

Input parameters:
* k - number of clusters.

## DBSCAN

Density-based spatial clustering of applications with noise ([DBSCAN](https://en.wikipedia.org/wiki/DBSCAN)).


# Evaluating clusters

## Silhouette

The [silhouette](https://en.wikipedia.org/wiki/Silhouette_(clustering)) value is a measure of how similar an object is to its own cluster (cohesion) compared to other clusters (separation). The silhouette ranges from -1 to 1, where a high value indicates that the object is well matched to its own cluster and poorly matched to neighboring clusters.