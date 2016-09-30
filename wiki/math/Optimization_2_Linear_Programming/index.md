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

## duality

## sensitivity

## complement


