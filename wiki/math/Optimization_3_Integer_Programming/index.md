---
layout: page
title: Optimization 3 Integer Programming
math_support: mathjax
---


## model

仅整数线性问题

线性规划**部分**决策变量为整数：纯整数 - 混合 - 0/1 型

去掉整数约束的松弛问题 与 原整数规划问题 的最优值的关系 （**前者更优**）

## 0-1 variable

- 有冲突、判断型约束，考虑0、1（、2）变量
- 约束条件求并集，用0、1变量
- 固定费用

## 割平面法

松弛问题 <=> 原问题 （何时等价？最优顶点满足整数约束）

若松弛最优解为整数则 halt

否则，找一个平面，在松弛最优附近割，去掉松弛最优解，留下所有可行解（整数）

纯整数时找到： $$\beta_k - \sum_j\alpha_{kj}x_j = 0$$

## branch and bound

可解决混合整数问题

用最优解分割，两个问题中对应约束是取等式（Proof）=> 对应某个分量在最优时得到整数解

