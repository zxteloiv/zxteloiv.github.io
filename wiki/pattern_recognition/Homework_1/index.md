---
layout: page
title: Homework 1
math_support: mathjax
---


**2.3** 证明：在两类情况下 $P(\omega_1 \vert x) + P(\omega_2 \vert x)=1$.

**证明**：显然成立

**2.4** 分别写出在以下两种情况  
(1)$P(x \vert \omega_1)=P(x \vert \omega_2)$
(2)$P(\omega_1)=P(\omega_2)$
下的最小错误率贝叶斯决策规则.

**解:** (1) 当 $P(x \vert \omega_1)=P(x \vert \omega_2)$ 时，$P(\omega_i \vert x) = P(x \vert \omega_i)P(\omega_i)/P(x) =\alpha P(\omega_i)$ , 其中 $\alpha$ 为正常数.
则最小错误率决策贝叶斯决策规则为：如果 $P(\omega_i)=\max_kP(\omega_k)$，则 $x \in \omega_i$
(2) 当 $P(\omega_1)=P(\omega_2)$ 时，$P(\omega_i \vert x) = P(x \vert \omega_i)P(\omega_i)/P(x) =\alpha P(x \vert \omega_i)$ , 其中 $\alpha$ 为正常数.
则最小错误率决策贝叶斯决策规则为：如果 $P(x \vert \omega_i)=\max_kP(x \vert \omega_k)$，则 $x \in \omega_i$

**2.10** 随机变量 $l(x)$ 定义为
$$
l(x)=\frac{p(x \vert \omega_1)}{p(x \vert \omega_2)},
$$
$l(x)$ 又称为似然比，试证明
(1)$E\{l^n(x)\vert\omega_1\}=E\{l^{n+1}(x)\vert\omega_2\}$
(2)$E\{l(x)\vert\omega_2\}=1$

**证明**:
(1) $$\begin{align}
E[l^n(x)\vert\omega_1]
  &= \int_{-\infty}^{+\infty}l^n(x)p(x \vert \omega_1)dx \\
  &= \int_{-\infty}^{+\infty}\frac{p^{n+1}(x \vert \omega_1)}{p^n(x \vert \omega_2)}dx \\
  &= \int_{-\infty}^{+\infty}\frac{p^{n+1}(x \vert \omega_1)}{p^{n+1}(x \vert \omega_2)}p(x \vert \omega_2)dx \\
  &= E[l^{n+1}(x)\vert\omega_2]
\end{align}
$$
(2) $$\begin{align}
E[l^n(x)\vert\omega_2]
  &= \int_{-\infty}^{+\infty}\frac{p(x \vert \omega_1)}{p(x \vert \omega_2)}p(x \vert \omega_2)dx \\
  &= \int_{-\infty}^{+\infty}p(x \vert \omega_1) dz  \\
  & = 1
\end{align}$$

**2.17** 若将矩阵$\Sigma^{-1}$写为
$$
\Sigma^{-1}=\begin{bmatrix}
h_{11} & h_{12} & \cdots & h_{1d} \\
h_{12} & h_{22} & \cdots & h_{2d} \\
\vdots & \vdots & \ddots & \vdots \\
h_{1d} & h_{2d} & \cdots & h_{dd} \\
\end{bmatrix}
$$
证明 Mahalanobis 距离平方为
$$
\gamma^2=\sum_{i=1}^d\sum_{j=1}^dh_{ij}(x_i-\mu_i)(x_j-\mu_j)
$$

**证明**: Mahalanobis 距离平方定义为 $\gamma^2 = (x-\mu)^T\Sigma^{-1}(x-\mu)$.
则易知当左乘 $(x-\mu)^T$ 时，对 $\Sigma^{-1}$ 做行线性组合，其中任意元素 $h_{ij}$ , 均会与 $x_i-\mu_i$ 相乘，再按列求和，得新的行向量 $\alpha$
当再右乘 $x-\mu$ 时，$\alpha$ 每一项 $\alpha_j$ 包含了 $h_{1j},h_{2j},\dots,h_{dj}$ 各项，均要与 $x_j-\mu_j$ 相乘，再相加可得一个数 $\gamma^2$.
可见在最终和式中，每一项 $h_{ij}$ 均有 $(x_i-\mu_i)(x_j-\mu_j)$ 的系数，而对所有的 $i,j$ 项求和，即有
$\gamma^2 = (x-\mu)^T\Sigma^{-1}(x-\mu)$.

**2.21** 对 $\Sigma_i=\Sigma$ 的特殊情况，指出在先验概率不等时，决策面沿 $\mu_i$ 与 $\mu_j$
点连线向先验概率小的方向移动.

**2.23** 二维正态分布，$\mu_1=(-1,0)^T,\mu_2=(1,0)^T,\Sigma_1=\Sigma_2=I,P(\omega_1)=P(\omega_2)$. 试写出负对数似然比决策规则.

**2.24** 在习题 2.23 中若 $\Sigma_1=\Sigma_2$,
$$
\Sigma_1=\begin{bmatrix} 1 & 1/2 \\ 1/2 & 1 \end{bmatrix},
\Sigma_2=\begin{bmatrix} 1 & -1/2 \\ -1/2 & 1 \end{bmatrix},
$$
写出负对数似然比决策规则.

**3.1** 设总体分布密度为 $N(\mu, 1), -\infty<\mu<+\infty$,
并设 $\mathscr X=\{x_1,x_2,\dots,x_N\}$, 分别用最大似然估计和贝叶斯估计计算 $\hat\mu$. 已知$\mu$的先验分布 $p(\mu)\sim N(0,1)$.

**3.7** 设 $\mathscr X=\{x_1,x_2,\dots,x_N\}$ 是来自 $p(x\vert\theta)$ 的随机样本，其中 $0\leq x\leq\theta$ 时，$p(x\vert\theta)=\frac{1}{\theta}$, 否则为0. 证明$\theta$的最大似然估计是 $\max_k\{x_k\}$.

给出 Parzen 窗估计的程序框图，并编写程序.
















