---
layout: page
title: Matrix_Analysis_-_1.3_linear_operator
math_support: mathjax
---


## 1-3. 线性算子
### 1. 定义

$\mathscr A: U^n\to V^m$ 的映射满足线性性质两条：数乘、求和
**例** : 求证 $\mathscr A: \mathbb R^n\to \mathbb R^m$, $\mathscr A(x)=A_{m\times n}\cdot x$ 为 $\mathbb R^n\to \mathbb R^m$ 的线性算子

### 2. (线性)算子对应矩阵
**引理** : $V^n$ 基 $\alpha_1,\dots,\alpha_n$ 则
$ \forall \alpha = (\alpha_1,\dots,\alpha_n)X\to\mathscr A(\alpha)= (\mathscr A\alpha_1,\dots,A\alpha_n)X$
故向量的像可以由基的像线性表示.

$\mathscr A:U^n\to V^m $ 为线性算子, $\alpha_1,\dots,\alpha_n$ 为 $U^n$ 基, $\beta_1,\dots,\beta_n$ 为 $V^m$ 基,
$$\mathscr A(\alpha_i)=(\beta_1,\dots,\beta_m)\begin{pmatrix}
a_{1i} \\
a_{2i} \\
... \\
a_{ni}
\end{pmatrix}\,, i=1\dots n$$

$$\therefore (\mathscr A(\alpha_1),\dots,\mathscr A(\alpha_n)) \\
\triangleq \mathscr A(\alpha_1,\dots,\alpha_n) 
= (\beta_1,\dots,\beta_m) \begin{bmatrix}
a_{11} & \dots & a_{1n} \\
\vdots & & \vdots \\
a_{m1} & \dots & a_{mn} 
\end{bmatrix} \triangleq  (\beta_1,\dots,\beta_m) \cdot A
$$
A 为此这两组基上的算子的对应矩阵

**例** : 给出算子和相应基, 求算子对应矩阵.
$\mathscr A:P_2[x]\to P_1[x]. \mathscr A(f)=(f)'$ 验证其为线性算子.
$f_1, f_2, f_3$ 为自然基, $g_1=2, g_2=1+x$ 为 $P_1[x]$ 基，求 $\mathscr A$ 在基偶 $((f_1,f_2,f_3), (g_1, g_2))$ 下对应矩阵.

### 3. 算子的性质
$\mathscr {A,B}:U^n\to V^m$ 且 $\alpha_1,\dots,\alpha_n$ 为 $U^n$ 基, $\beta_1,\dots,\beta_n$ 为 $V^m$ 基, 并且 $ \mathscr A \leftrightarrow A, \mathscr B \leftrightarrow B $,
则它满足
1. $\mathscr A + \mathscr B \leftrightarrow A + B$
2. $\lambda \mathscr A \leftrightarrow \lambda A $

所以，空间上的算子仍然构成了线性空间

若 $PAQ=B \Rightarrow A=P^{-1}BQ^{-1}$,
$$
\mathscr A(\alpha_1,\dots,\alpha_n)
= (\beta_1,\dots,\beta_n)A
=(\beta_1,\dots,\beta_n)P^{-1}BQ^{-1},$$

$$
\therefore \mathscr A(\alpha_1,\dots,\alpha_n)Q = (\beta_1,\dots,\beta_n)P^{-1}B
$$
而 $(\alpha_1,\dots,\alpha_n)Q, (\beta_1,\dots,\beta_n)P^{-1}$ 又可以视作新的基偶

B 矩阵好算时（对角阵 etc.) 可以用此方法化简

*Hint* : 后面会提到同一个线性变换在不同基偶下的对应矩阵相似.


