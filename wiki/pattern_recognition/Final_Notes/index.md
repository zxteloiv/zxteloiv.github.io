---
layout: page
title: Final Notes
math_support: mathjax
---


# Bayesian Decision

## decision theory
min error:

$$
p(e) = \int_x p(e, x) dx = \int_x p(e \mid x) p(x) dx
$$

min risk:

$$
R = \int_x R(\alpha(x) \mid x) p(x) dx
$$

for every single x, find the minimized error / risk to make the whole loss minimized.

## normal distribution

$$
p({\bf x} \mid \theta) = \frac{1}{\sqrt[d]{2\pi} \,\vert\Sigma\vert^{1/2}}
\exp(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu))
$$

properties:

- linear: $$A^TX \sim N(A^T\mu, A^T\Sigma A)$$
- whiten: $$A_w = \Phi \Lambda^{-1/2}$$

# non-parametric estimation

region:

$$
P = \int_R p(x') dx' \simeq \frac{k}{n}
$$

where:

- k: samples in the region
- n: the number of total samples

pointwise：

$$
p_n(x) = \frac{P}{V} \simeq \frac{k}{nV}
$$

for a series convergent to p(x) (a single point), there're 3 conditions:

$$
\lim_{n\to\infty} V_n = 0 \\
\lim_{n\to\infty} k = \infty \\
\lim_{n\to\infty} k/n = 0 \\
$$

two way to fix a variable:

- k-means, $$k = \sqrt{n}$$
- parzen window, $$V_n = 1/h_n^d$$

# parametric estimation

## MLE / BE:

key: 

$$
p(x \mid c_j, D) = \int p(x \mid \theta) p(\theta \mid D) d\theta
$$

MLE find the max theta that $$p(\theta\mid D) = \frac{p(D \mid \theta)\, p(\theta)}{Z}$$

BE find max $$p(\theta\mid D) = \int p(D\mid\theta)p(\theta)d\theta$$

## EM: 

in order to maximize $$L(\theta) = \log P(Y\mid\theta)=\log(\int_Z P(Y\mid Z,\theta)P(Z\mid\theta)\,dZ\,)$$

iteratively, using Jensen inequality to find a lower bound for $$L(\theta) - L(\theta^i)$$

to maximize the lower bound of $$\theta$$ gives the Q function

$$
\theta^{(i+1)} = \arg\max_\theta Q(\theta, \theta^{(i)})= \arg\max_\theta E_Z[\ln P(Y, Z\mid \theta)\mid Y, \theta^{(i)}]
$$

# SVM

## original model

$$
\min \frac{1}{2}\Vert w\Vert^2 \\
s.t.\, y_i(w^Tx_i+b) \ge 1
$$

using lagrange multiplier method, this is equivalent to

$$
\min_{w,t} \max_{\alpha} \frac{1}{2}\Vert w\Vert^2 + \sum_i\alpha_i (1 - y_i(w^Tx_i+b))\\

s.t.\, \alpha_i \ge 0
$$

> note the max part, in order to minimize it, discuss about both the condition violation and alpha value

then the lagrange duality may apply

## relaxed model:

$$
\min \frac{1}{2}\Vert w\Vert^2 + C\sum_i\xi_i\\
s.t.\, y_i(w^Tx_i+b) \ge 1 - \xi_i
$$

# feature extraction

## PCA

- way1: minimize the variation of projected samples
- way2: minimize the difference(least square) of reconstructed samples

using way1: 

$$
\min E(w) = var(w^Tx) = w^T\Sigma w\\
s.t. \, \Vert w\Vert = 1
$$

using way2:

$$
E(B, Y, c) = \sum_n\sum_i \Vert x_i^{(n)} - \tilde x_i^{(n)} \Vert^2 \\
\tilde x^{(n)} = c + \sum_j^m y_j^{(n)}b^{(j)} \\
\therefore E = tr((X-BY)^T(X-BY))
$$

## LDA

to minimize the loss within classes and maximize the loss between classes

$$
J_1 = \Vert m_1 - m_2 \Vert^2 \\
J_2 = S_1 + S_2, \, S_i=\frac{1}{n}\sum_{j\in c_i} (w^Tx_j - m_i)^2 \\
\max J = J_1 / J_2
$$

## KPCA

replace the reconstruction with kernels (???)

$$
\tilde x = w^T\phi(x)
$$

## LLE

using neighbor to reconstruct the data

1\. find w that $$\min_w\Vert x_i-\sum_{j\in N(i)}w_{ij}x_i\Vert^2$$

equ. to (???)

$$
\min_w w^TQw \\
s.t. w^Te = 1 \\
where, \, Q = G^TG \\
G = [x_i - x_{i,1}, ..., x_i - x_{i,k}] \\
e = [1,1,...,1] \\
\therefore w = \frac{Q^{-1}e}{e^TQ^{-1}e}
$$

2\. find embedding y that (???)

$$
\min_y\sum_i\Vert y_i - \sum_{j\in N(i)}w_{ij}y_j\Vert^2 = y^TMy \\
where, \, M_{ij} = \delta_{ij} - w_{ij} - w_{ji} + \sum_k w_{ki}w_{kj} \\
$$

result is eigenvectors of M (???)

# feature selection

- criterion
- searching methods: branch and bound, gene, forward/backward greedy algorithms

# clustering

## mixture

start from the mixture density:

$$
p(x\mid\theta) = \sum_i P(w_i)p(x\mid\theta_i)
$$

## hierarchical

every points is a cluster -> every points is assigned into a big cluster

- find the minimal dist within a class that is the smallest
- use the max dist between class

## spectral

generalized graph nodes and edges for points and similarity.

simplest: L = D - W

# linear discriminant functions

## perceptron

- perceptron: $$J = \sum(-a^Ty)$$
- add a margin to the boundary: $$a^Ty > b$$
- single sample and batch sample

## relaxation

$$
J = \frac{1}{2}\sum_y \frac{(a^Ty-b)^2}{\Vert y\Vert^2}
$$

## MSE

$$
J = \sum_i(a^Ty_i - b)^2
$$

solve:

- LMS
- pseudo-inverse

## model selection

- no free lunch
- adaboost: 

$$
w, G(x) \\
e = sum(w .* G(x)) \\
\alpha = 1/2 \ln \frac{1-e_{min}}{e_{min}} \\
w = \frac{w .* \exp(-\alpha y G(x))}{Z} \\
... \\

... \\
G = sign(\sum_m \alpha_m G_m(x))
$$

## neural networks

... omitted













