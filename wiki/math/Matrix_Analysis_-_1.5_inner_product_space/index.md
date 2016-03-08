---
layout: page
title: Matrix_Analysis_-_1.5_inner_product_space
math_support: mathjax
---


## 1-5. 内积空间

两个例子：
1. $ (f,g) = \int_a^bf(x)g(x)dx $ 其中 $ f,g \in \mathbb C[a,b] $ 
2. $(x, y) = \Sigma_ix_iy_i=x^Ty$

### 1. 内积
给定线性空间 $(V^n,\mathbb R)$,
定义映射 $\forall x, y \in V^n, \lambda \in \mathbb R;  x,y\to(x,y)$
满足4条:
1. 可交换 $(x,y) = (y,x)$
2. $(x+y,z)=(x,z)+(y,z)$
3. $(\lambda x,y) = \lambda(x,y)$
4. 非负 $(x,x)\geqslant0 \text{ 且 } (x,x)=0 \Leftrightarrow x=0$

**例** : 验证例子 1 为内积定义

欧式空间：线性空间＋内积
内积空间性质：
1.长度: $x\in V^n, \vert x\vert = \sqrt{(x,x)}$

2.夹角: $\theta_{x,y} = \arccos \frac{(x,y)}{\vert x\vert \vert y\vert}$

3.度量矩阵: $V^n$ 基 $e_1,\dots,e_n$.
$$\forall x,y\in V^n, x=(e_1,\dots,e_n)\begin{pmatrix}x_1\\ \vdots \\ x_n\end{pmatrix}, y=(e_1,\dots,e_n)\begin{pmatrix}y_1\\ \vdots \\ y_n\end{pmatrix}$$
$$(x,y) = (x_1e_1+\dots+x_ne_n,y_1e_1+\dots+y_ne_n) = \Sigma\Sigma x_iy_j(e_i,e_j)$$
$$\therefore (x,y)=X^TAY=X^T\begin{pmatrix}
(e_1,e_1) & \cdots & (e_1,e_n) \\
\vdots & & \vdots \\
(e_n,e_1) & \cdots & (e_n,e_n)
\end{pmatrix}Y$$
A 为度量矩阵.
若基 $e_1,\dots,e_n$ 是标准正交基，A 可为单位阵

$0\leqslant (x,x)=X^TAX \Rightarrow$ A 是正定矩阵

**例** : 基(I) $e_1,\dots,e_n$ 度量矩阵为 A，基(II) $e_1',\dots,e_n'$ 度量矩阵为 B.
且 $(e_1',\dots,e_n') = (e_1,\dots,e_n)C$.
讨论 A, B, C 的关系.

$$
B=(e_1',\dots,e_n')^T(e_1',\dots,e_n') \\
= ((e_1,\dots,e_n)C)^T(e_1,\dots,e_n)C \\
= C^T(e_1',\dots,e_n')^T (e_1,\dots,e_n)C \\
= C^TAC
$$
所以 A 与 B 合同.(同一空间不同基下的度量矩阵合同)

### 2. 正交
$(x,y)=0$
正交向量组(不含零元)一定线性无关.
*Hint* : 设 $\alpha_1, \dots, \alpha_n$ 为正交向量组.
设
$$ \Sigma_{i=1}^n\lambda_i\alpha_i=0 \\
\Rightarrow (\alpha_1, \Sigma_{i=1}^n\lambda_i\alpha_i) = 0 \\
\Rightarrow \lambda_1(\alpha_1, \alpha_1) = 0 \\
\Rightarrow \lambda_1 = 0
$$
类似，将等式与其他向量组内向量作内积，可得其他常数为0

$\star$标准正交基: Gram-Schmidt 方法
**例** : 给出一组基: $\alpha_1, \dots, \alpha_n \in V^n$. 求标准正交基.
正交规范化过程：
(1) 正交化:
$$
\beta_1 = \alpha_1 \\
\beta_2 = \alpha_2 - \frac{(\alpha_2,\beta_1)}{(\beta_1,\beta_1)}\beta_1 \\
\vdots \\
\beta_n = \alpha_n - \Sigma_{i=2}^{(n-1)}\frac{(\alpha_n,\beta_i)}{(\beta_i,\beta_i)}\beta_i
$$

(2) 规范化:
$$\gamma_i = \frac{\beta_i}{\vert\beta_i\vert}, i = 1\dots n$$

标准正交基与任意向量作内积，可得向量用该正交基表示的坐标
$$
\begin{pmatrix}x_1\\ \vdots \\x_n\end{pmatrix}
= \begin{pmatrix}(x,e_1)\\ \vdots \\(x,e_n)\end{pmatrix}
$$







