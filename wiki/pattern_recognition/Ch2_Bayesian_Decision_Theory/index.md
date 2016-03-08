---
layout: page
title: Ch2_Bayesian_Decision_Theory
math_support: mathjax
---


lvwang@nlpr.ia.ac.cn

## intro

$$P(\omega_1)$$, $$P(\omega_2)$$ two classes prob.

by convention, P is discrete, p is continuous

to compute $$P(\omega_1 \vert \bf{x})$$ <- posterior (data known then class)
bayesian formula: ....

concepts: ...

posterior + (prior (no info)) -> max likelihood

A posterior could serve as prior for the next posterior (争议)

(likelihood sum for multiple classes may > 1 only for continuous distribution)

## scheme 1 min error

two-class error: 

$$
P(e) = \int P(e \vert x) p(x) dx \\
where \\
P(e \vert x) = P(\omega_1 \vert x) \text{ if x is } \omega_2
$$

min error

in practice, we may use a decision boundary rather than the integral of a complex distribution.

## scheme 2 min risk

sometimes, prior is too influential compared to the likelihood (observation),

so we introduce risk for every decision: $$ risk: \lambda_{ij} = \lambda(\alpha_i, \omega_j)$$

expected loss:

$$
R(\alpha)=\int R(\alpha(x) \vert x) p(x) dx \\
where \\
R(\alpha_i \vert x) = \sum_{j=1}^c \lambda_{ij} P(w_j \vert x)
$$

min risk: $$\min_\alpha R(\alpha \vert x) $$ c may be larger than the number of class （拒识）

$$\lambda$$ could be some hyperparameters to be learnt in other ways (like using incremental learning)

some $$\lambda$$ makes the min-risk degraded to min-error

**learning the risk**: ??? **(section 2.4)**
(paricle group/gene / blabla..) rather than gradient descent

## determinant func, decision boundary

广义似然函数: $$g_i(x), i = 1, \dots, c$$, many different forms of $$g$$
decision: $$\arg \max_i g_i(\bf{x})$$

decision boundary: (abstract: man not a line, a curve, or ....)

determinant func: g(x) <= g1(x) - g2(x) 

## to determin decision boundary or determinant func, in gaussian distribution

gaussian: blablabla (or _bell function_)
and its statistics: mean, square error, entropy, blablabla...

and multivariate gaussian: blabla

1. $$\mu$$ and $$\Sigma$$ determine the distribution
2. 等密度点 $$r^2=blabla$$
3. 不相关等价于独立
4. 边缘、条件分布正态
5. **线性变换正态**: $$y=A^Tx \Rightarrow p(y) \sim N(A^t\mu, A^t\Sigma A)$$
6. 线性组合正态:

**白化变换**: (ellipse -> round)

$$
A_\omega = \Phi \Lambda^{-1/2} \\
\begin{align}
A^T_w\Sigma A_w &=\Lambda^{-1/2}\Phi^T\Sigma\Phi\Lambda^{-1/2} \\
&=\Lambda^{-1/2}\Lambda\Lambda^{-1/2} \\
&=I \\
\end{align}
$$

### bayesian in gaussian

determinant: choose $$g_i(x) = \ln p(x|w_i) + \ln P(w_i)$$

**case 1**: $$\Sigma_i=\sigma^2I,i=1,2,\dots,c$$
可得二类决策面 $$w^T(x-x_0)=0$$
...

**case 2**: $$\Sigma_i=\Sigma, i=1,2,\dots,c$$
...

**case 3**: $$\Sigma_i$$ is arbitrary
...
determinant form: ...
2-class decision boundary: ... (超二次曲面)

#### classification error

...


















