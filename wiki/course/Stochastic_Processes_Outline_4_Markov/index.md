---
layout: page
title: Stochastic Processes Outline 4 Markov
math_support: mathjax
---


### 状态离散、时间离散

定义 Markov Chain: 无后效性 => 一步转移矩阵

=> Chapman-Kolmogorov 方程（先 k 步转移，再 m 步转移）

=> n 步转移
$$P(n)=[P(1)]^n$$

=> 遍历性 极限转移概率矩阵 与 i 无关
$$ \lim_{n\to\infty}p_{ij}=p_j $$

=> 判定方法
- 存在一个k，有 k 步转移矩阵全大于0
- 且满足 $$\pi=(p_1, .., p_N) \\ \pi=\pi P \\ \sum p_i=1$$

由上式解出 pi 可得方程

### 状态离散、时间连续

#### 转移矩阵

亦有转移矩阵 => Chapman-Kolmogorov 方程

#### 速率函数（转移矩阵的导数相关）
考虑状态转移矩阵的导数
$$\begin{align}
\lim_{\Delta t\to0}\frac{p_{ij}(t+\Delta t)-p_{ij}(t)}{\Delta t}
&=\lim_{\Delta t\to0}\frac{\sum_kp_{ik}(t)(p_{kj}(\Delta t)-\sigma_{kj})}{\Delta t} \\
&=\sum_kp_{ik}(t)\lim_{\Delta t\to0}\frac{p_{kj}(\Delta t)-\sigma_{kj}}{\Delta t} \\
&=\sum_kp_{ik}(t)q_{kj}(t)
\end{align}
$$

定义的 q 称为速率函数，实际为 t=0 时的导数

#### 遍历性

定义、判定方式 类似上面

### 独立增量过程

定义：X(0) = 0 ，增量 为 相互独立 的随机变量

=> 增量符合柏松分布，为柏松过程
=> 增量复合正态分布，为~~正太~~维纳过程

### 状态连续、时间连续

定义该过程的 分布函数 时需要使用极限
（条件概率中，条件的状态等于特定值的概率为0，通过在特定值附近一个区间的方式取极限）



