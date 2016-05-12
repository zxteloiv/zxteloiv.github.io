---
layout: page
title: Ch8 data clustering
math_support: mathjax
---


## intro

> clusters can be different in different tasks for the same dataset

different methods

- ...
- ...
- ...

## various similarity metrics

... blabla ...

## mixture function 

designate domain-specific assumptions

Assumption: data is generated on mixture function

$$
p(x \vert \theta) = \sum_{j=1}^c p(x \vert w_j, \theta_j) P(w_j)
$$

### MLE for parameters of density function

#### gaussian density \mu unknown

#### gaussian density \mu, \sigma, prior unknown

### bayesian estimation

blabla ...

## hierarchical clustering

- bottom-up
  - min
  - max
- top-down

**bottom-up:**

two samples per time, take the two with smallest distance as a cluster, update the dist between new cluster and others using the min/max distance 

## spectral clustering

- PCA
- LDA(linear determinant analysis
- spectrum embedding (in manifold learning)
- ...

adjacency matrix -> generalized to -> similarity matrix (weighted)

- full connectivity
- local
  - k-nn
  - $\epsilon$-radius
  
find a cut of a graph that makes some criteria optimal.

### min binary cut

to minimize:

$$
\min_A cut(A, \bar A) = W(A, \bar A) = \sum_{i\in A, j\in\bar A} w_{ij}
$$

however, the result ofter just only separate a single outlier point as a cluster, and others in a total form a cluster

-> normalize the cut with similar size for both clusters

- use cardinality (count of vertices in a set)
- use volume (sum of degree of vertices in a set)

to solve this in the following secitons

### laplace matrix properties

- ...
- ...
- ...
- ...
- number of the eigen vectors of a laplace matrix w.r.t the eigen value 0, equals to the number of connected subgraphs of the graph 


#### explanation

- classical
- Ncut
- Ng's algo.






















