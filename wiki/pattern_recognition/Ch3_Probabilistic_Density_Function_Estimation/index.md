---
layout: page
title: Ch3_Probabilistic_Density_Function_Estimation
math_support: mathjax
---


## MLE

key point: to estimate $p(x\vert \omega_i)$, using parameterized or non-parameterized

TODO: 指数加速分布

**parameterized estimation** in many place:

1. given classifier structure / function, estimate parameters using samples
2. given distribution, estimate parameters (Gaussian)
3. other stat learning functions

MLE: presumptions: $\theta$ is fixed ... blabla i.i.d blabla bla ...

> sometimes, i.i.d is hard to complement, while i.i.d. under some assumption

(??? what if hidden variable hard to find)

1. likelihood: $p(D\vert\theta) = \sum_{k=1}^np(x_k\vert\theta)$
2. maximize the function above, (partial derivative all zero)

**case 1**: gaussian dist, $\mu$ unknown
(???? $\frac{d}{d\mu} (x-\mu)^T\Sigma^{-1}(x-\mu) $)

**case 2**: 1-d gaussian dist, $\mu, \sigma^2$ unknown
...

**case 3**: high-dimension gaussian dist, $\mu, \sigma^2$ unknown
...

## Bayesian Estimation

MLE gives a value for a parameter, but BE gives a distribution. 

1. decide a prior dist. form
2. decide likelihood dist. form
3. compute posterior (based on prior & likelihood)
4. compute bayesian estimator

## Feature Dimension

1. 降维
2. 参数共享（共享协方差）
3. 参数平滑（shrinkage、正则化）

## EM

complement in next lecture




















