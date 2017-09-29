---
layout: default
title: Dimensionality reduction
---


# Dimensionality Reduction

## Linear discriminant analysis

Linear discriminant analysis (LDA) is a generalization of Fisher's linear discriminant, a method used in statistics, pattern recognition and machine learning to find a linear combination of features that characterizes or separates two or more classes of objects or events. The resulting combination may be used as a linear classifier, or, more commonly, for dimensionality reduction before later classification.

https://www.r-bloggers.com/computing-and-visualizing-lda-in-r/

## Principal Component Analysis

PCA is mathematically defined as an orthogonal linear transformation that transforms the data to a new coordinate system such that the greatest variance by some projection of the data comes to lie on the first coordinate (called the first principal component), the second greatest variance on the second coordinate, and so on.
Another way to characterise the principal components transformation is therefore as the transformation to coordinates which diagonalise the empirical sample covariance matrix.


https://www.r-bloggers.com/computing-and-visualizing-pca-in-r/



## Non-negative matrix factorization

[Non-negative matrix factorization](https://en.wikipedia.org/wiki/Non-negative_matrix_factorization) (NMF or NNMF), also non-negative matrix approximation is a group of algorithms in multivariate analysis and linear algebra where a matrix V is factorized into (usually) two matrices W and H, with the property that all three matrices have no negative elements. This non-negativity makes the resulting matrices easier to inspect.

If we add additional orthogonality constraint on H {\displaystyle H} H, i.e., H H T = I {\displaystyle HH^{T}=I} HH^{T}=I, then the above minimization is mathematically equivalent to the minimization of K-means clustering ).


## Vector quantization

[Vector quantization](https://en.wikipedia.org/wiki/Vector_quantization) (VQ) is a classical quantization technique from signal processing that allows the modeling of probability density functions by the distribution of prototype vectors. It was originally used for data compression. It works by dividing a large set of points (vectors) into groups having approximately the same number of points closest to them. Each group is represented by its centroid point, as in k-means and some other clustering algorithms.


## t-SNE

[t-distributed stochastic neighbor embedding (t-SNE)](https://en.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding) is a nonlinear dimensionality reduction technique that is particularly well-suited for embedding high-dimensional data into a space of two or three dimensions, which can then be visualized in a scatter plot. Specifically, it models each high-dimensional object by a two- or three-dimensional point in such a way that similar objects are modeled by nearby points and dissimilar objects are modeled by distant points.  The technique can be implemented via Barnes-Hut approximations, allowing it to be applied on large real-world datasets.

https://cran.r-project.org/web/packages/Rtsne/Rtsne.pdf
https://nlml.github.io/in-raw-numpy/in-raw-numpy-t-sne/


