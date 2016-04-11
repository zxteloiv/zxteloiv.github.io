---
layout: page
title: Homework 2
math_support: mathjax
---


### 1.1

有四个来自于两个类别的二维空间中的样本，其中第一类的两个样本为 $(1,4)^T$ 和 $(2,3)^T$，第二类的两个样本为 $(4,1)^T$ 和 $(3,2)^T$ 。这里，上标 $T$ 表示向量转置。假定采用规范化增广样本表示形式，假设初始的权向量 $a=(0,1,0)^T$ ，$a$ 的第三维对应齐次坐标，且梯度更新步长 $\eta_k$ 固定为 1。试利用批处理感知器算法求解线性判别函数 $g(y)=a^Ty$ 的权向量。（注意请采用规范化增广样本表示形式）

**解**: 设原样本分别为 $x_1,x_2 \in \omega_1, x_3,x_4 \in \omega_2$ ，对其进行规范化得

$$
y_1 = \begin{pmatrix}
1 \\ 4 \\ 1
\end{pmatrix},
y_2 = \begin{pmatrix}
2 \\ 3 \\ 1
\end{pmatrix},
y_3 = \begin{pmatrix}
-4 \\ -1 \\ -1,
\end{pmatrix},
y_4 = \begin{pmatrix}
-3 \\ -2 \\ -1
\end{pmatrix}
$$

第一步，使用初始化 $a=(0,1,0)^T$, 求得

$$
a^Ty_1 = 4, a^Ty_2=3,a^Ty_3=-1,a^Ty_4=-2
$$

错分集合 $Y = \{y_3, y_4\}$，且 $J = \sum_{y\in Y} (-a^Ty) $ ，则更新权重向量：

$$
\begin{align}
a &= a - \eta_1\frac{\partial J}{\partial a} \\
  &= a + \sum_{y\in Y}y \\
  &= a + y_3 + y_4 = (-7, -2, -2)^T
\end{align}
$$

第二步，利用 $a = (-7, -2, -2)^T$ 寻找错分向量：

$$
\begin{align}
a^Ty_1 &= -7-8-2 &= -17 &\le 0 \\
a^Ty_2 &= -14 -6 - 2 &= -22 &\le 0 \\
a^Ty_3 &= 28 + 2 + 2 &= 32 &\ge 0 \\
a^Ty_4 &= 21 + 4 + 2 &= 27 &\ge 0
\end{align}
$$

错分集合 $Y = \{y_1, y_2\}$ 则更新权重向量 $ a = a + y_1 + y_2 = (-4, 5, 0)^T $.

第三步，求错分集合：

$$
\begin{align}
a^Ty_1 &= -4 + 20 &\ge 0 \\
a^Ty_2 &= -8 + 15 &\ge 0 \\
a^Ty_3 &= 16 - 5 &\ge 0 \\
a^Ty_4 &= 12 - 10 &\ge 0
\end{align}
$$

错分集合为 $Y=\emptyset$ 则 $\eta_3 \sum_{y\in Y}y = 0$，算法终止。

求解得权向量：$ a = (-4, 5, 0)^T $.

### 1.2

对于多类分类情形，考虑 one-vs-all 技巧，即构建 $c$ 个线性判别函数：

$$
g_i(x) = w_i^Tx + w_{i0}, i = 1, 2, \dots, c
$$

此时的决策规则为：对 $j \ne i$, 如果 $g_i(x) > g_j(x)$, x 则被分为 $\omega_i$ 类。
现有三个二维空间内的模式分类器，其判别函数为：

$$
g_1(x) = -x_1 + x_2 \\
g_2(x) = x_1 + x_2 - 1 \\
g_3(x) = -x_2
$$

试画出决策面，指出为何此时不存在分类不确定性区域。

**解**: 分类面如图所示：

<div><img src="resources/E2AB90225E6E93B64B7F5F668A8A0BC5.jpg" alt="FullSizeRender.jpg"></div>

由于每个类的判别函数在特定点上总有一个最大者，故整个平面中不存在有歧义或未分类的点，不存在分类不确定区域。


























