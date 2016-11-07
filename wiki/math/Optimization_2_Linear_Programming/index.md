---
layout: page
title: Optimization 2 Linear Programming
math_support: mathjax
---


## model

non-negative constraints: benifit algo. design (in simplex)

q1:

$$
\max 2x_1 + x_2 \\
s.t. 5x_2 \leq 15 \\
6x_1 + 2x_2 \leq 24 \\
x_1 + x_2 \leq 5 \\
x_1 \ge 0, x_2 \ge 0
$$

q2:

$$
\min \sum_{ij}c_{ij}x_{ij} \\
s.t. \sum_j x_{ij}=a_{i} \\
\sum_i x_{ij}=b_{j} \\
x_{ij} \ge 0, \\
i \in [1,m], j \in [1,n]
$$

generalized model: ...

canonical: unequality -> equality

added variable: (phisical meaning) remaining resource quantity, e.g. $$x_3 = 15 - 5x_2$$

标准型假定

1. m 行线性无关
2. n > m

否则 无解 或 无需优化

$$
AX = [B \vert N][X_B \vert X_N]^T, \exists B^{-1}, \text{suppose }X_N= 0, X_B \text{ is decidable}
$$

## low-dimension

linear programming: constraint by constraint, feasible region get smaller (set intersection)

1. 可行解凸集
2. 顶点不在两点连线上
3. 顶点有限

## basic algo

基阵 -> 基本解 -> 基本可行解

## simplex

典式：右边非负，约束条件有单位阵

退化问题：表右端有0（基变量取值0）
无穷最优解：达到最优之后，表下方行有0（进基依然最优解），中间状态下下方为0并不能说明什么，继续迭代找最优解

初始：人为对标准型添加人工变量（每个条件）n+m 个变量，但要求全部出基：

- 大 M 法
- 两阶段法

若人工变量无法出基：原问题无解

## duality

原问题最优性条件 => 对偶问题的可行性条件

## sensitivity

## complement


