---
layout: post
title:  "Hopfield 网络复习笔记"
date:   2015-11-17 14:00
tags: learning hopfield neural_network 
categories: note learning
math_support: mathjax
---

### 离散 Hopfield 网络

结构：各个神经元互相联系，但并不与自己相连，故是一种反馈网络

输出：$$v_i(t+1) = f(\sum_{j\ne i}(w_{ij}v_j(t)) + b_i)$$

运行方式：

- 异步：一时刻仅一个神经元变化
- 同步：一时刻部分或全部神经元一起变化

#### 异步运行

1. 初始化
2. 选一个神经元
3. 更新输出
4. 判断是否稳定

稳定条件：

1. 权重对称
2. 不连接自身

稳定条件满足时，能量函数降低：

$$

E = -\frac{1}{2}\sum_i\sum_{j\ne i}w_{ij}v_iv_j+\sum_ib_iv_i

$$

即，对每个神经元的能量函数求和：

$$


E_i = -\frac{1}{2}\sum_{j\ne i}w_{ij}v_iv_j + b_iv_i


$$

由于能量函数有界、递减，故最终必稳定

#### 同步运行

$$

\mathbf v(t+1) = f(\mathbf W \mathbf v(t) + \mathbf b)

$$

### 连续 Hopfield 网络

**网络结构**: 基本不变

**对应于电子电路的网络结构**: 电路图、基尔霍夫电流定律

**能量函数及其稳定性分析**:

能量函数：

$$
\begin{aligned}
E &= - \frac{1}{2} \sum_i\sum_j w_{ij}x_ix_j + \sum_i\theta_ix_i +
\sum_i\frac{1}{\tau_i}\int_0^{x_i}f^{-1}(x)dx \\
&= -\frac{1}{2}x^TWx+x^\theta+\sum_i\frac{1}{\tau}\int_0^{x_i} f^{-1}(x)dx
\end{aligned}
$$

可得能量函数单调递减（dE/dt <= 0)，故系统稳定

