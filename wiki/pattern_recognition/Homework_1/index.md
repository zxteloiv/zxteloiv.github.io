---
layout: page
title: Homework 1
math_support: mathjax
---


### 2.3

证明：在两类情况下 $P(\omega_1 \vert x) + P(\omega_2 \vert x)=1$.

**证明**：由于 P(\omega \vert x) 的意义为概率，则应积分为1 即 $\int P(\omega\vert x)d\omega = 1$，只有两类时即有$P(\omega_1 \vert x) + P(\omega_2 \vert x)=1$

### 2.4

分别写出在以下两种情况  
(1) $P(x \vert \omega_1)=P(x \vert \omega_2)$  
(2) $P(\omega_1)=P(\omega_2)$  
下的最小错误率贝叶斯决策规则.

**解:** (1) 当 $P(x \vert \omega_1)=P(x \vert \omega_2)$ 时，$P(\omega_i \vert x) = P(x \vert \omega_i)P(\omega_i)/P(x) =\alpha P(\omega_i)$ , 其中 $\alpha$ 为正常数.

则最小错误率决策贝叶斯决策规则为：如果 $P(\omega_i)=\max_kP(\omega_k)$，则 $x \in \omega_i$

(2) 当 $P(\omega_1)=P(\omega_2)$ 时，$P(\omega_i \vert x) = P(x \vert \omega_i)P(\omega_i)/P(x) =\alpha P(x \vert \omega_i)$ , 其中 $\alpha$ 为正常数.

则最小错误率决策贝叶斯决策规则为：如果 $P(x \vert \omega_i)=\max_kP(x \vert \omega_k)$，则 $x \in \omega_i$

### 2.10

随机变量 $l(x)$ 定义为

$$
l(x)=p(x \vert \omega_1)/p(x \vert \omega_2),
$$  

$l(x)$ 又称为似然比，试证明

(1) $E\{l^n(x)\vert\omega_1\}=E\{l^{n+1}(x)\vert\omega_2\}$   
(2) $E\{l(x)\vert\omega_2\}=1$

**证明**:
(1)

$$\begin{align}
E[l^n(x)\vert\omega_1]
  &= \int_{-\infty}^{+\infty}l^n(x)p(x \vert \omega_1)dx \\
  &= \int_{-\infty}^{+\infty}{p^{n+1}(x \vert \omega_1)}/{p^n(x \vert \omega_2)}dx \\
  &= \int_{-\infty}^{+\infty}{p^{n+1}(x \vert \omega_1)}/{p^{n+1}(x \vert \omega_2)}p(x \vert \omega_2)dx \\
  &= E[l^{n+1}(x)\vert\omega_2]
\end{align}
$$

(2)

$$\begin{align}
E[l^n(x)\vert\omega_2]
  &= \int_{-\infty}^{+\infty}{p(x \vert \omega_1)}/{p(x \vert \omega_2)}p(x \vert \omega_2)dx \\
  &= \int_{-\infty}^{+\infty}p(x \vert \omega_1) dz  \\
  & = 1
\end{align}$$

### 2.17

若将矩阵$\Sigma^{-1}$写为

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

### 2.21

对 $\Sigma_i=\Sigma$ 的特殊情况，指出在先验概率不等时，决策面沿 $\mu_i$ 与 $\mu_j$ 点连线向先验概率小的方向移动.

**解**: 决策面为: 

$$
\begin{align}
g_i(x) &= g_j(x) \\
\Leftrightarrow -{1}/{2}(x-\mu_i)^T\Sigma^{-1}(x-\mu_i) + \ln P(\omega_i) &= -{1}/{2}(x-\mu_j)^T\Sigma^{-1}(x-\mu_j) + \ln P(\omega_j) \\
\end{align}
$$

显然，当 $P(\omega_i)$ 减小时，$g_j(x)$ 值将相对增大，则所占特征空间的区域也将增大，故决策面将向着 $\omega_i$ 方向移动.

### 2.23

二维正态分布，$\mu_1=(-1,0)^T,\mu_2=(1,0)^T,\Sigma_1=\Sigma_2=I,P(\omega_1)=P(\omega_2)$. 试写出负对数似然比决策规则.

**解**: 由题意有，$P(x\vert\omega_i)\sim N(\mu_i,\Sigma_i)$ 故

$$ \begin{align}\ln P(x\vert\omega_i) 
&= -{1}/{2}(x-\mu_i)^T\Sigma_i^{-1}(x-\mu_i)-{1}/{d}\ln2\pi-{1}/{2}\ln\vert\Sigma_i\vert \\
&= -{1}/{2}(x-\mu_i)^T(x-\mu_i)-{1}/{d}\ln2\pi
\end{align}
$$

负对数似然比为

$$
\begin{align}
-\ln{P(x\vert\omega_1)}/{P(x\vert\omega_2)}
  &= -\ln P(x\vert\omega_1) +\ln P(x\vert\omega_2) \\
  &= {1}/{2}((x-\mu_1)^T(x-\mu_1) - (x-\mu_2)^T(x-\mu_2)) \\
  &= {1}/{2}(\parallel x-\mu_1 \parallel^2- \parallel x-\mu_2 \parallel^2)
\end{align}
$$

而由于 $P(\omega_1)=P(\omega_2)$ 则 $\ln(P(\omega_1)/P(\omega_2)) = 0$ ，所以所求决策规则为

当 $(\parallel x-\mu_1 \parallel^2- \parallel x-\mu_2 \parallel^2) < 0$ 取 $\omega_1$，否则取 $\omega_2$.  
$\Leftrightarrow (x_1+1)^2+x_2^2 - (x_1-1)^2-x_2^2 = 4x_1 < 0$ 时取 $\omega_1$，否则取 $\omega_2$.  
$\Leftrightarrow x_1 < 0$ 时取 $\omega_1$，否则取 $\omega_2$  

### 2.24

在习题 2.23 中若 $\Sigma_1\neq\Sigma_2$,
$$
\Sigma_1=\begin{bmatrix} 1 & 1/2 \\ 1/2 & 1 \end{bmatrix},
\Sigma_2=\begin{bmatrix} 1 & -1/2 \\ -1/2 & 1 \end{bmatrix},
$$
写出负对数似然比决策规则.

**解**: 由于 $\Sigma_1\neq\Sigma_2$ ，则判别函数中对应项不能相互抵消，则决策规则变为：
$ -\ln P(x\vert\omega_1) + P(x\vert\omega_2) < 0 \Rightarrow x \in \omega_1$ 否则 $x\in\omega_2$

由于恰好有 $\vert\Sigma_1\vert = \vert\Sigma_2\vert$ 和 $\Sigma_1^{-1}=4/3\Sigma_2,\Sigma_2^{-1}=4/3\Sigma_1$

则决策规则为

$$
(x-\mu_1)^T\Sigma_1^{-1}(x-\mu_1) - (x-\mu_2)^T\Sigma_2^{-1}(x-\mu_2) < 0 \Rightarrow x \in \omega_1
$$

代入化简得

$$
2x_1 - x_1x_2 < 0 \Rightarrow x \in \omega_1
$$

### 3.1

设总体分布密度为 $N(\mu, 1), -\infty<\mu<+\infty$,
并设 $\mathscr X=\{x_1,x_2,\dots,x_N\}$, 分别用最大似然估计和贝叶斯估计计算 $\hat\mu$. 已知$\mu$的先验分布 $p(\mu)\sim N(0,1)$.

**解**: 用最大似然计算：

$$
\begin{align}
\hat \mu
  &= \arg\max_\mu p(\mathscr X\vert\mu) = \arg\max_\mu \prod_k p(x_k\vert\mu) \\
  &= \arg\max_\mu \prod_k {1}/{\sqrt{2\pi}}\exp(-{(x_k-\mu)^2}/{2}) \\
l(\mu)
  &= \ln(\prod_k {1}/{\sqrt{2\pi}}\exp(-{(x_k-\mu)^2}/{2})) \\
  &= \sum_{k=1}^N{1}/{\sqrt{2\pi}}-\sum_{k=1}^N {(x_k-\mu)^2}/{2} \\
{d}/{d\mu}l(\mu)
  &= \sum_{k=1}^N (x_k - \hat \mu) \\
  &= -N\hat \mu + \sum_{k=1}^N x_k = 0 \\
\Rightarrow \hat \mu
  &= \sum_{k=1}^N x_k / N
\end{align}
$$

用贝叶斯估计计算：

$$
\begin{align}
\hat \mu = {N}/{N + 1}({1}/{N}\sum_{k=1}^Nx_k)
\end{align}
$$

### 3.7

设 $\mathscr X=\{x_1,x_2,\dots,x_N\}$ 是来自 $p(x\vert\theta)$ 的随机样本，其中 $0\leq x\leq\theta$ 时，$p(x\vert\theta)=\frac{1}{\theta}$, 否则为0. 证明$\theta$的最大似然估计是 $\max_k\{x_k\}$.

**证明**: 

$$
P(\mathscr X \vert \theta) = \prod_{k=1}^N p(x_k\vert\theta) = \theta^{-N} \\
s.t. \forall x_k,  0 \leq x_k \leq \theta, k = 1, 2, \dots, N
$$

当上述约束不满足，似然值将 = 0，显然比上式得数要小，故为了让似然最大，上述约束应当都满足，则 $\hat\theta = \max_k\{x_k\}$.

**给出 Parzen 窗估计的程序框图，并编写程序.**


















