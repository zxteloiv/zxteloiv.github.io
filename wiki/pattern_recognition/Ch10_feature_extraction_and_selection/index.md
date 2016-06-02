---
layout: page
title: Ch10 feature extraction and selection
math_support: mathjax
---


# feature extraction

## curse of dimension

...

## linear extraction

extraction: $$y=F(x)$$

linear: $$y=W^Tx$$

### PCA 方差最大

$$w = \arg\max var(W^Tx)$$

第二个特征需与第一个不相关

### PCA 重建数据角度

blabla

## non-linear

### kernel PCA

Schölkopf, B., Smola, A., & Müller, K.-R. (1998). Nonlinear component analysis as a kernel eigenvalue problem. Neural Computation, 10(5), 1299–1319.

### LLE

???

## others

ICA

Laplacian Eigenmaps: similarity in low-dimension
LLE: reconstruction struct in low-dimension

# feature selection

select a subset of features

## evaluation criterion

class difference: $$J_{ij}$$

- monotone w.r.t loss
- addable when features are independent
- ??

criterior

1\. similar with LDA

2\. prop-based: prefer more data-separable features  (be careful about linear combination of other data-separable features)

3\. entropy-based: posterior prop entropy: less is better

## selection optimization algorithm

search-based:

1. filter: generize, 
2. wrapper

branch and bound:

greedy search: (increment / decrement / both)

random search: (gene algorithm)




