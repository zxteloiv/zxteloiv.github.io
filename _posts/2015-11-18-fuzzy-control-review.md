---
layout: post
title:  "模糊控制复习笔记"
date:   2015-11-18 19:27
tags: learning fuzzy_theory fuzzy_set 
categories: note learning
math_support: mathjax
---

### 引言

Note: intuition:

* 概率论主要反映的是客观上的自然的不确定性
* 模糊性主要是人为的主观理解上的不确定性

### 理论基础

> 集合论基础
> 
> 集合 元素 论域(研究的对象总和) 空集 子集

### 模糊集合

隶属度函数

$$\mu_A:X\to [0,1]$$

> 今天天气：冷、热、不冷、不热

离散形式、连续形式

Zadeh 表示法

$$A = \int_X\mu_A(x_i)/x \text{ or } \sum_{x_i\in X}\mu_A(x_i)/x_i$$

模糊集合基本运算

1. 相等 $$ A = B \leftrightarrow \mu_A(x) = \mu_B(x)$$
2. 包含/子集 $$A\subset B \leftrightarrow \mu_A(x)\leq\mu_B(x)$$
3. 模糊空集 $$A=\emptyset \leftrightarrow \mu_A(x) = 0 $$
4. 并(取大) $$C = A\cup B, \mu_C=max(\mu_A(x),\mu_B(x)) = \mu_A(x)\vee\mu_B(x)$$
5. 交(取小) $$C = A\cap B, \mu_C=\mu_A\wedge\mu_B$$
6. 补(用1减)

模糊集合性质

1. 幂等 $$A\cup A = A, A\cap A = A$$
2. 交换、结合 
3. 吸收 $$A\cup (A\cap B) = A$$
4. 分配 $$A\cup (B\cap C) = (A\cup B)\cap(A\cup C)$$
5. 对偶(德摩根)
6. 两极

### 模糊关系

$$ R(X,Y)=\{((x,y), \mu_R(x,y)) \vert (x,y)\in X\times Y\} $$

二元、Cartesian Product

**关系表示**：隶属度函数（连续）、模糊矩阵

> e.g. 父母，子女 长得 相似

* R o S = max - min 合成 $$\mu_{R\circ S}(x,z)=\vee_{y\in Y}(\mu_R(x,y)\wedge\mu_S(y,z))$$
* R o S = max - 乘积合成 $$\mu_{R\circ S}(x,z)=\vee_{y\in Y}(\mu_R(x,y)\cdot\mu_S(y,z))$$

> e.g. 合成：父母子女相似，父母与祖父母相似 合成 得子女与祖父母相似

### 模糊逻辑

#### 语言变量

* x: 变量名称
* T(x): 语言变量值
* X: 论域
* G: 产生x值名称的语法规则
* M: 与各值含义有关的语义规则

$$x:\to G \to T(x) \to M \to X$$

年龄->语法(G)->语言变量值(年幼、年长)->语义规则(M)->论域(X)

M 规定了隶属度函数：年幼->年龄范围

**语气算子**: 词的前缀改变隶属度(e.g.)

$$

\begin{aligned}
\mu_{\text{extremely }A} &= \mu_A^4, \\
\mu_{\text{very} A} &= \mu_A^{1.25}, \\
\mu_{\text{a bit } A} &= \mu_A^{0.25}
\end{aligned}

$$

#### 模糊蕴含关系

* 蕴含：if ... then ... 记作 $$A\to B$$.
* 命题：
  * 二值命题 为真(1)或假(0)，
  * 模糊命题: [0, 1] 上取值
* 记法
  * if A then B: $$A\to B$$
  * if A then B else C: $$(A\to B)\cup(\bar A\to C)$$
  * if A and B then C: $$A\times B \to C$$

**模糊蕴含的运算**

* 模糊蕴含最小运算 Mamdani
  * $$R = A\to B = A\times B = A^TB = \int\mu_A(x)\cap \mu_B(y) /(x,y)$$
  * A 为 m * 1 矩阵，乘的结果为 m * n 矩阵
* 模糊蕴含积运算 Larsen
  * $$R = A\to B = A\times B = A^Tg B = \int\mu_A(x)\mu_B(y) /(x,y)$$

蕴含概念：

* A 对应一种m元模糊集 $$A, \mu_A(x)$$
* B 对应一种n元模糊集 $$B, \mu_B(x)$$
* 蕴含关系：m * n 元模糊集，对应隶属度函数$$A\times B \to \mu_{A\to B}(x)$$

### 模糊推理方法

**单前提单规则**

* 前提 事实(模糊集) ＋ 前提 规则(模糊蕴含)
* 结论 (模糊集): $$B' = A' \circ (A \to B)$$
* $$A'$$ 为一元时，$\circ$ 所表示的关系合成: 行向量 X 矩阵

可以先计算出一部分，可得一个常数:

$$
\begin{aligned}
\mu_{B'}(y) &= \vee _x(\mu_{A'}(x)\wedge\mu_A(x)\wedge\mu_B(y) )\\
&= (\vee_x(\mu_{A'}(x)\wedge\mu_A(x)))\wedge\mu_B(y) \\
&= \omega \wedge \mu_B(y)
\end{aligned}
$$

**多前提单规则**

* 前提 事实 （模糊集＋模糊集）＋前提 规则（模糊蕴含）
* 结论(模糊集): $$C' = (A'\times B')\circ(A\times B\to C)$$

可以先计算出一部分

**多前提多规则** (MISO)

* 前提 事实（模糊集，模糊集）＋ 前提 规则（蕴含1 蕴含2)
* 结论(模糊集): $$C'=(A'\times B')\circ(R_1\cup R_2)​$$

可以先计算出一部分

**MIMO**

使用规则库