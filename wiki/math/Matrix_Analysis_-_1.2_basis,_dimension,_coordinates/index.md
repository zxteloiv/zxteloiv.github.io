---
layout: page
title: Matrix_Analysis_-_1.2_basis,_dimension,_coordinates
math_support: mathjax
---



## 1-2. 基、维数、坐标

### 1. 线性表示

... omitted ...

### 2. 线性无关

**例** : 求证 $ f_1=1, f_2=x, f_3=x^2 $ 线性无关, $1, \sin x$ 线性无关
**例2** : 求证 $ g_1=1+x,g_2=1-x,g_3=x^2 $ 线性无关

### 3. 基
不多、不少，极大无关组. 维数 $ n \rightarrow V^n $

**例** : 求证 $f_1, f_2, f_3$ 为 $P_2[x]$ 的一组基
**例** : 求证 $g_1, g_2, g_3$ 为 $P_2[x]$ 的一组基
*Hint* : 按定义两条

### 4. 坐标
记 $$
\beta = (\alpha_1,\dots,\alpha_n)\begin{pmatrix}
x_1 \\ 
... \\
x_n
\end{pmatrix}$$ 这种分块表示较方便

$$X=\begin{pmatrix}
x_1 \\ 
... \\
x_n
\end{pmatrix}$$
为 $\beta$ 在基 $\alpha_1,\dots,\alpha_n$ 下的坐标. 故可得 $\beta \leftrightarrow X\in \mathbb R^n$, 研究传统向量空间的向量即可研究 $\beta$

**例** : 求证: $P_n[x] = \{f(x)=...\}$ 是 n+1 维
*Hint* : 找一组基，基维数为 n+1 即可

**例** : $$M_{2\times2}=\{\begin{pmatrix}
a_{11} & a_{12} \\
a_{21} & a_{22} 
\end{pmatrix}
\vert \, a_{11}, a_{12}, a_{21}, a_{22}\in\mathbb R\}$$ 求维数

### 5. 过渡矩阵
给定基 $\alpha_1,\dots,\alpha_n$ 和基 $\beta_1,\dots,\beta_n$, 有

$$\beta_i=(\alpha_1,\dots,\alpha_n)\begin{pmatrix}
c_{1i} \\
c_{2i} \\
... \\
c_{ni}
\end{pmatrix},$$

即每个 $\beta_i$ 有基 $\alpha_1,\dots,\alpha_n$ 下坐标
$$\begin{pmatrix}
c_{1i} \\
c_{2i} \\
... \\
c_{ni}
\end{pmatrix}$$
过渡矩阵: $C=(C_{ij})_{n\times n}$ 为从基 $\alpha_1,\dots,\alpha_n$ 到基 $\beta_1,\dots,\beta_n$ 的过渡矩阵

**例** : 会求过渡矩阵.

**Note** : 两组不好的基 -> 可以由自然基当桥梁

$$
(\beta_i) = (\alpha_k)\cdot C = (\alpha_k)\cdot A^{-1}B \Leftarrow \left\{\begin{matrix}
(\alpha_i)=(e_1, \dots, e_n)A  \\ 
(\beta_i)=(e_1, \dots, e_n)B
\end{matrix}\right.
$$




