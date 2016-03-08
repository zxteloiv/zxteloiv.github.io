---
layout: page
title: Matrix_Analysis_-_Outline_1_Geometry
math_support: mathjax
---


记录关于矩阵理论的一些逻辑主线。

### 1 推广

推广 向量 为 任意集合，得到 [线性空间](quiver-note-url/12F3DF5A-1F33-4953-B8C2-0E217BEB6231) 满足 **8个条件**

> 数乘 条件中有 数域 概念

空间 中引出
**基**: 一组空间中的向量，**不多**、**不少**

对应 [basis](quiver-note-url/132394FD-6B84-40F9-B87D-68EEF36B2353) 得到 坐标，可以表示任意向量

不同基 之间有 过渡矩阵 相联系

> 子空间 概念  
> 子空间 有 直和 等概念

### 2 抽象算子

[算子](quiver-note-url/EF311FF9-8527-4C97-B2E8-DD88EAE8DFB3) 是集合之间的映射

线性算子 满足 线性条件 $$\mathscr A(ax+by) = a\mathscr A(x) + b\mathscr A(y)$$

> 0算子、数乘、微分、积分 等都是 线性算子  
> 空间同构 $\Leftrightarrow$ 空间存在 同构算子(一一对应)

基 可表示任意向量，故研究 基 在 算子 下的像.

基像 可以由 目标空间 的一组基 表示.

表示关系 得到 **算子对应一组基偶的矩阵表示**

> 变换 的连续作用 与 矩阵 的对应关系一一对应

### 3 线性变换

从 一个空间 映射到 同一空间 的线性算子 称为 [线性变换](quiver-note-url/68A84243-5ED3-47A3-90ED-7F906AF725D2)

> 线性变换满足 交换律、结合律、左右数乘分配率

线性变换 在 不同的基 下 对应的 矩阵 相似 $ B = C^{-1}AC $，C 为过渡矩阵

> 证明关键，变换：$\mathscr A(basis')=\mathscr A(basis)C=(basis')B$

研究秩，与以前类似关系和结论

线性变换的 **特征值** 和 **特征向量**
同样由 $\mathscr A(x)=\lambda x$ 开始推导

得到
- 特征值 $\leftrightarrow$ 矩阵的特征值
- 特征向量 $\leftrightarrow$ 矩阵的特征向量 和 基 的线性组合

### 4 内积空间

定义 [内积](quiver-note-url/774661B3-8B80-40CE-90EC-9CAFCAF302C6) 满足 4条

由 内积 定义
- **长度**或**模** $\vert x\vert = \sqrt{(x,x)}$
- **角度**

> Cauthy-Schwarz 不等式的证明  
> 要点：构造 $(u,u) = (x+ty,x+ty) \ge 0$

由 基 构成 **度量矩阵** 得 $(x,y)=X^TAY$

> 度量矩阵 对称、正定  
> 不同基的 度量矩阵 相合同 $B=C^TAC$

### 5 正交

由内积为0，定义向量[正交](quiver-note-url/A60F2759-CFCF-453E-8E02-ACB004269A02)

一组基正交 得到 正交基，再 标准化 得到 **标准正交基**

标准化重点 减去 前面所有正交向量上的投影 $$b_i =a_i-\sum_{k=1}^{i-1}\frac{(a_i,b_k)}{(b_k,b_k)}b_k$$

### 6 等积变换

线性变换 的 像 的 内积 等于 原始向量的内积，就是等积变换。

> 此时变换对应的矩阵为 **正交阵**: $A^TA=AA^T=I$

常用的正交变换
旋转变换：沿 xOy 平面 顺时针旋转，对应x、y维度 上 如下矩阵
$A=\begin{pmatrix}\cos\theta & \sin\theta \\ -\sin\theta & \cos\theta \end{pmatrix}$
左乘一个向量，将该向量旋转。若更高维，每次只旋转一个平面，其他维度为单位1

镜像变换：变换矩阵 $H=I-2\omega\omega^T$，其中 $\omega$是与超镜面垂直的一个单位向量
> 证明:用原像、镜像的向量之差的模长，求得单位向量w的倍数


