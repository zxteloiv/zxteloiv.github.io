---
layout: page
title: Stochastic_Processes_-_1.2 distribution
math_support: mathjax
---


## 1-2 随机过程的分布

### 1. 一维分布

$ \forall t \in T, X(t), F(x, t) = P(X(t) \leqslant x) $ 称为随机变量的一维分布. 

$ \{F(x, t), t \in T\} $ 一维分布函数族. 

**例**：X 的分布律

| x | 1 | 2 | 3 | 
|---:|:---:|:---:|:---:|
| P | 0.2 | 0.3 | 0.5 |

随机过程 $ X(t) = X \sin^2 t, t \in N^+ $. 求分布函数

**解**：

| $X(t)$ | $1\sin^2t$ | $2\sin^2t$ | $3\sin^2t$ |
| ------ | ---------- | ---------- | ---------- |
| P      | 0.2        | 0.3        | 0.5        |   

$$
F(x, t) = P(X(t) \leqslant x) = \left\{\begin{matrix}
 & 0 & x < \sin^2t \\ 
 & 0.2 & \sin^2t \leqslant x < 2\sin^2t & \\ 
 & 0.5 & 2\sin^2t \leqslant x < 3\sin^2t & \\ 
 & 1 & 3\sin^2t \leqslant x & 
\end{matrix}\right.
$$

**例2** : $X~N(\mu, \sigma^2), X(t) = 2X+t, t\in \mathbb R$ 求 $F(x, t), f(x, t)$

**例3** : $$X: f(x)=\left\{\begin{matrix}
& 2x & 0 \leq x \leq 1 \\
& 0 & 其他
\end{matrix}\right.$$

### 2. 多维分布
$\forall t_1, t_2, ..., t_n \in T, (X(t_1), ..., X(t_n)) $ 是 n 维随机变量.
$ F(x_1, ..., x_n; t_1, ..., t_n) = P(X(t_i) \leq x_i, \forall i = 1..n) $ 是随机过程的多维分布

**例4** : X 分布律如下：

|x | 1 | 2|
|--|--| --|
| P | 0.2 | 0.8 |

$X(t) = Xt, t\in \mathbb N^+$. 求$F(x_1, x_2, t_1, t_2)$
**Hint** :
$$ \forall t_1, t_2 \in \mathbb N^+, X(t_1) = t_1X, X(t_2)=t_2X \\
F=P(t_1X \leq x_1, t_2X \leq x_2) = P(X\leq \frac{x_1}{t_1}, X\leq\frac{x_2}{t_2}) = ......
$$

**例5** : 求$F(x_1, x_2, 3, 4)$
**Hint** : $$F = P(X\leq\frac{x_1}{3}, X\leq\frac{x_2}{4})=......$$

### 3. 联合分布
$ \{X(t), t\in T\}, \{Y(t), t\in T\}$, T 为相同集合.
$\forall t_i\in I^n, t_j\in J^m$
$$F(x_1,...,x_n,y_1,...,y_n,t_1,...,t_m,t'_1,...,t'_m) = \\ P(X(t_i)\leq x_i, Y(t'_j)\leq y_j, \forall i \in 1..n, \forall j \in 1..m)$$
称为两个随机过程的联合分布


