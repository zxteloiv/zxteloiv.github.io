---
layout: page
title: Matrix Analysis 1.4 linear transform
math_support: mathjax
---


## 1-4. 线性变换
### 1. 定义
$\mathscr A:V^n\to V^n, \mathscr A(\alpha_1,\dots,\alpha_n) = (\alpha_1,\dots,\alpha_n)\cdot A$, 此时 A 为方阵.

**性质** : $\alpha_1,\dots,\alpha_n $ 与 $ \beta_1,\dots,\beta_n $ 为 $V^n$ 的基，且 $(\beta_1,\dots,\beta_n) = (\alpha_1,\dots,\alpha_n)\cdot C$
$\mathscr A(\alpha_1,\dots,\alpha_n) = (\alpha_1,\dots,\alpha_n) A$
$\mathscr B(\beta_1,\dots,\beta_n) = (\beta_1,\dots,\beta_n) B$
则 $B = C^{-1}AC$
故 同一个线性变换在不同基下对应的矩阵相似

**例** : $\mathscr A$ 在 $V^4$ 基 $\alpha_1, \alpha_2, \alpha_3, \alpha_4$ 下矩阵
$$A = \begin{pmatrix}
1 & 2 & 0 & 1 \\
2 & 0 & 3 & 1 \\
1 & 3 & 4 & 5 \\
2 & 1 & 1 & 1
\end{pmatrix}$$

$V^4$ 基 $\beta_1, \beta_2, \beta_3, \beta_4$ 有：
$$
\begin{cases}
\beta_1 = \alpha_1 \\
\beta_2 = \alpha_1+\alpha_2 \\
\beta_3 = \alpha_1+\alpha_2 + \alpha_3 \\
\beta_4 = \alpha_1+\alpha_2 + \alpha_3 + \alpha_4 
\end{cases}
$$
求 $\mathscr A$ 在基 $\beta_1, \beta_2, \beta_3, \beta_4$ 下矩阵.

*Hint* : 利用过渡矩阵比定义方便

### 2. 对角化
$\mathscr A$ 为 $V^n$ 上线性变换，若有 $\lambda\in\mathbb R, 0\neq x\in V^n$ 使 $\mathscr A(x) = \lambda x$
则 特征值 $\lambda$, 特征向量 x
$\mathscr A(\alpha_1,\dots,\alpha_n) = (\alpha_1,\dots,\alpha_n)A$ 记 $x = (\alpha_1,\dots,\alpha_n)X$, 即 X 为坐标.

$$\mathscr A(x) = \mathscr A(\alpha_1,\dots,\alpha_n)X = (\alpha_1,\dots,\alpha_n)AX \\
= \lambda x = \lambda (\alpha_1,\dots,\alpha_n)X = (\alpha_1,\dots,\alpha_n) \lambda X \\
\therefore AX=\lambda X
$$
故求线性变换的特征值，可以转变为求矩阵特征值，对应特征向量为基乘坐标

**例**: $\mathscr A$ 在基 $\alpha_1, \alpha_2, \alpha_3$ 下矩阵 $$A = \begin{pmatrix}
2 & 1 & 1 \\
1 & 2 & 1 \\
1 & 1 & 2 
\end{pmatrix}$$.
求: (1) $\mathscr A$ 的特征值和特征向量. (2) 求基 $\beta_1, \beta_2, \beta_3$ 使 $\mathscr A$ 在基下矩阵为对角阵.
*Hint* : 
(1) $\vert \lambda E - A \vert = 0 \Rightarrow \lambda_1 = 4, \lambda_2=\lambda_3=1$
$\text{when } \lambda_1 = 4, (4E-A)X=0 \Rightarrow X_1=(1, 1, 1)^T$
故 $\lambda_1$对应特征向量 $x_1 = (\alpha_1, \alpha_2, \alpha_3)X = \alpha_1+ \alpha_2+ \alpha_3$
类似求出其他特征向量...

(2) $C^{-1}AC=\Lambda = \begin{pmatrix}4 & & \\ & 1 & \\ & & 1 \end{pmatrix}$
所以取 $(\beta_1, \beta_2, \beta_3) = (\alpha_1, \alpha_2, \alpha_3)C$ ... etc




