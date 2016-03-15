---
layout: page
title: Matrix_Analysis_1.6_orthogonal_transform
math_support: mathjax
---


## 1-6. 正交变换
### 1. 定义
$\forall\alpha,\beta\in V^n, (\mathscr A(\alpha),\mathscr A(\beta)) = (\alpha,\beta)$ 则该变换为正交变换
可验证正交变换保长度、保角度

定理: 对欧式空间，下列等价:
1. $\mathscr A$ 为正交变换
2. $\forall\alpha\in V^n, (\mathscr A(\alpha),\mathscr A(\alpha)) = (\alpha,\alpha)$
3. 任意一个标准正交基 $e_1,\dots,e_n$ 的像 $\mathscr A(e_1),\dots,\mathscr A(e_n)$ 也为标准正交基
4. $\mathscr A$ 在任意标准正交基下的矩阵 P 满足 $P^TP = PP^T = E$. (P为正交矩阵)

**例** : 求一个正交阵 P 使第一行为 (1/3, 2/3, 2/3).
*Hint* : 第2、3行与之正交，本身也正交，且为单位向量.
求方程解时可以直接猜两个正交解，避免后续又要正交化.

**例** : $(e_1',\dots,e_n') = (e_1,\dots,e_n)P\Rightarrow$ P为正交阵.

**例** : $V^n$ 为欧式空间. 基 $e_1,\dots,e_4$ 的度量矩阵
$$
A = \begin{pmatrix}
2 & 1 & 2 & 1 \\
1 & 1 & 0 & 1 \\
2 & 0 & 5 & 1 \\
1 & 1 & 1 & 3
\end{pmatrix}
$$
构造一组标准正交基.
*Hint* : 正交化过程中需要计算内积，可以直接由度量矩阵查得.

### 2. 旋转变换

[image_missing]

$$
\mathscr A(\vec i) = \cos\theta\cdot\vec i - \sin\theta\cdot\vec j \\
\mathscr A(\vec j) = \sin\theta\cdot\vec i + \cos\theta\cdot\vec j \\
\therefore \mathscr A(\vec i, \vec j) = (\vec i, \vec j)\begin{pmatrix}
\cos\theta & \sin\theta \\
-\sin\theta & \cos\theta
\end{pmatrix} = (\vec i, \vec j) A
$$

可各种方法验证这是正交变换.

推广 $R_{ij}$ 为沿 iOj 平面顺时针旋转.
$$
R_{ij} = \begin{pmatrix}
1 & & & & & & & & & & \\
 & \ddots & & & & & & & & & \\
 & & 1 & & & & & & & & \\
 & & & \cos\theta & \dots & \dots & \dots & \sin\theta & & & \\
 & & & \vdots & 1 & & & \vdots & & & \\
 & & & \vdots & & \ddots & & \vdots & & & \\
 & & & \vdots & & & 1 & \vdots & & & \\
 & & & -\sin\theta & \dots & \dots & \dots & \cos\theta & & & \\
 & & & & & & & & 1 & & \\
 & & & & & & & & & \ddots & \\
 & & & & & & & & & & 1 
\end{pmatrix}_{n\times n}
$$
可验证它为正交阵.
例如: 3维空间在 xOy 平面旋转 
[image_missing]

**例** : 将 $X=(1,2,3)^T$ 旋转变换与 $e_1=(1,0,0)^T $ 同方向向量，求变换矩阵.
*Hint* : 猜最后旋转结果为 $(\sqrt{14},0,0)^T$
第一步沿 xOy 旋转
[image_missing]

几何法找到 $\theta, \cos\theta, \sin\theta$
或代数法
$$
\left\{\begin{matrix}
\cos\theta = \frac{x_1}{\sqrt{x_1^2+x_2^2}} \\
\sin\theta = \frac{x_2}{\sqrt{x_1^2+x_2^2}}
\end{matrix}\right.
$$

### 3. 镜像变换
$$
(\mathscr A(\vec i), \mathscr A(\vec j)) = (\vec i, \vec j)
\begin{pmatrix}
1 & 0 \\
0 & -1
\end{pmatrix} = (\vec i, \vec j)H \\
H = I - 2ww^T
$$
w 为一个与镜面垂直的向量. 可验证 H 为正交阵，变换为正交变换.

想法:
[image_missing]

任意给镜面，一定可以找到 w 与之垂直
$\mathscr A(\zeta) = \eta$
按图可知 $\zeta - \eta$ 在 w 上的投影 = 2倍 $\zeta$ 在 w 上的投影 = $2w^T\zeta$
*Note* : $(w, b) = \vert w\vert\vert b\vert\cos\theta = 1\cdot\vert b\vert\cos\theta$ 可得上述最后等号
$$
\therefore \zeta - \eta = (2w^T\zeta)w = 2ww^T\zeta \\
\therefore \eta = (I - 2ww^T)\zeta \\
\mathscr A(\zeta) = \eta = (I - 2ww^T)\zeta = H \cdot \zeta
$$

**例**: 用镜面变换将 $\zeta = (0, 3, 0, 4)^T$ 变为与向量 $e_1 = (1, 0, 0, 0)^T 同方向的向量，并求出变换矩阵.
*Hint* : 四维向量不好画，就随便画图
[image_missing]

找一个与 $\zeta$ 方向上的单位向量（为了与 $e_1$ 一样长)
则找一个 w 如下（我们并不关心镜面）
$$
w = \frac{(\frac{\zeta}{\vert\zeta\vert} - e_1)}{\vert \frac{\zeta}{\vert\zeta\vert} - e_1 \vert} \\
\therefore H = I - 2ww^T = ...
$$
镜面得到的像 $\mathscr A(\zeta) = e_1\vert\zeta\vert$ 相当于把长度移到第一维上.

**Note** : 空间中最镜像：要么对一个平面作镜像；若仅以一个方向作镜子，则一般要求原像、像、镜子向量三者共面.


