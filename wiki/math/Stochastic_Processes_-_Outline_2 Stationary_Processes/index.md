---
layout: page
title: Stochastic_Processes_-_Outline_2 Stationary_Processes
math_support: mathjax
---


### 概念

严平稳过程（略）

弱平稳过程
- 均值函数为常数
- 相关函数仅与时间差有关

弱平稳不一定是严平稳

### 各态历经性

实际是在时间轴上的一个平均，定义

$$
\lt X(t) \gt = \lim_{T \to \infty} \frac{1}{2T}\int_{-T}^TX(t)dt
$$

若满足 $$\begin{align} \lt X(t) \gt &= m_X \\ \lt X(t)X(t+\tau) \gt &=R_X(\tau) \end{align}$$

则称随机过程有(期望|相关函数)各态历经性

### 谱密度与线性系统

自相关函数的傅立叶变换为谱密度 $$S_X(\omega)=\mathscr F[X(t)]$$

线性系统 输出的谱密度 $$S_Y(\omega)=\vert H(i\omega) \vert^2S_X(\omega)$$

输出的期望 $$EY(t)=m_X\int_0^\infty h(\lambda)d\lambda$$

输出的自相关函数除了用谱密度做 傅立叶逆变换 之外也可以
$$
R_Y(\tau)=\int_0^\infty\int_0^\infty R_X(\lambda_2-\lambda_1-\tau)
h(\lambda_1)h(\lambda_2)
d(\lambda_1)d(\lambda_2)
$$



