---
layout: page
title: Stochastic_Processes_1.3_statistical_characteristics
math_support: mathjax
---


## 1-3 数字特征
思想：用数字特征（函数）的相应性质研究随机过程.
### 1. 均值函数
$\{X(t),t\in T\}$ 均值函数：$\forall t\in T, E(X(t)) = m_x(t) = \mu_x(t)$ (需存在)

### 2. 方差函数
方差函数：$D_x(t) = D(X(t)) $
标准差函数：$\sigma_x(t) = \sqrt{D_x(t)}$

### 3. 均方值函数
$$\Psi(t)=E(X^2(t))$$

### 4. 自相关函数
$$\forall t_1, t_2 \in T, E(X(t_1)X(t_2)) \triangleq R_X(t_1, t_2)$$

### 5. 协方差函数
$$Cov(X(t_1),X(t_2)) = C_X(t_1, t_2) $$

### 6. 相关系数(函数)
$$=\frac{Cov}{\sigma^2}$$

**例2** : X分布率

|  x| 1  |  2 | 3 |
|---|--- | ---| --|
|P  | 0.5| 0.1|0.4|

$X(t) = X\sin t$, 求均值函数、自相关函数.

解: $ E(X(t)) = E(X\sin t) = \sin tE(x) = ... $
$R_X(t_1, t_2) = E(X(t_1)X(t_2)) = E(X^2\sin t_1 \sin t_2) = ... $

**例3** :随机相位正弦波 $X(t) = a\cos(\omega t + \Phi), t \in \mathbb R$，其中 $a, \omega$为常数，$\Phi \sim U[0, 2\pi]$
求: $m_X(t), R_X(t_1, t_2)$
解: 
$$
\Phi\sim U[0,2\pi], \\ f(\phi)=\left\{\begin{matrix}
 & \frac{1}{2\pi}, & 0 \leqslant \phi \leqslant 2\pi \\
 & 0, & others
\end{matrix}\right.
$$
得 $$
m_X(t) = E(a\cos (\omega t + \Phi)) \\
= \int_{0}^{2\pi}a\cos (\omega t + \phi)  \frac{1}{2\pi}d\phi \\
= 0
$$
和 
$$\begin{align}
R_X(t_1, t_2) &= \frac{a^2}{2\pi}\int_0^{2\pi}\cos(\omega t_1 + \phi)\cos(\omega t_2 + \phi)d\phi \\
&= \frac{a^2}{2}\cos(\omega(t_2-t_1))
\end{align}
$$
*可验证此为平稳过程*

**例4** : $X(t)=A\cos t + B\cos t, t\in \mathbb R. $ 且 $ A \sim N(\mu, \sigma^2), B\sim N(\mu, \sigma^2) $.
求 $\mu_X(t), R_X(t_1, t_2) $
解: $\mu_X = \mu(\cos t + \sin t)$ （可验证亦为常数）
$R_X(t_1, t_2)=E(x(t_1)x(t_2))$ （一元时间差函数）

### 7. 互相关函数
Given $\{X(t), t\in T\}, \{Y(t), t \in T\}$,
互相关函数 $R_{XY}(t_1, t_2) = E(X(t_1)Y(t_2))$

**例5** : $X(t)=A\cos t, Y(t) = A\sin t, A\sim N(\mu, \sigma^2)$. 求 $R_{XY}(t_1, t_2)$

**例6** : $X. f(x) = \left\{\begin{matrix} & 2x, & 0 \leqslant x \leqslant 1 \\ & 0, & others \end{matrix}\right. $.
$X(t)=2X+t, t\in \mathbb R$. 求 $f(x_1, t)$

解: 
$$ \begin{align}
F(x_1, t) 
&= P(X(t)\leqslant x_1) \\
&= P(2X+t \leqslant x_1) \\
&= P(X \leqslant \frac{x_1-t}{2}) \\
&= \int_{-\infty}^{(x_1-t)/2}f(x)dx
\end{align}
$$
所以 
$$ \begin{align}
f(x_1,t) 
&= \frac{\partial f(x_1, t)}{\partial x_1} \\
&= \frac{1}{2} f(\frac{x_1-t}{2}) \\
&= \left\{\begin{matrix}
& \frac{1}{2}(x_1-t), & 0 \leqslant \frac{x_1-t}{2} \leqslant 1 \\
& 0, & others
\end{matrix}\right.
\end{align}
$$



