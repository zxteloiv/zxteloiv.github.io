---
layout: post
title:  "抽象代数笔记 - 1 - 集合论"
date:   2015-04-25 15:51
tags: learning, math, abstract_algebra, group_theory, set_theory
categories: note learning
math_support: katex
---

### 1. 集合

#### 1.1 定义

朴素集合论中，对“集合”不给出定义。

空集：不包含任何元素的集合。

又可以定义为：

空集：任何对于此集合的元素所作的全称命题均为真。

全称命题：形如"对于所有的XX，某命题为真"   
存在命题：形如"存在XX，使某命题为真"

其他：子集、真子集

集合A与B相等：$$ A \subset B $$ 且 $$ A \supset B $$

#### 1.2 基本运算

* 并集
* 交集

并集和交集满足定律

* 分配律
    * $$ A \cap (B \cup C) = (A \cap B) \cup (A \cap C) $$
    * $$ A \cup (B \cap C) = (A \cap B) \cap (A \cup C) $$
* 结合律
* 交换律

其他集合 

* 差集
* 补集
* 全集

德摩根定律: $$ (A \cap B)^c = A^c \cup B^c $$, $$ (A \cup B)^c = A^c \cap B^c $$

笛卡尔积(卡式积): 定义为集合：$$ A \times B = \{(a, b) \mid a \in A, b \in B\}  $$

不交并(disjoint unioin): 并集时对相同元素做区分, 即 $$ A \sqcup B = (A \times \{0\}) \cup (B \times \{1\}) $$

幂集: 子集的集合

#### 1.3 关系

关系：定义在 A 上的关系，是 A 与自身卡式积的子集

偏序关系：A 上的偏序关系是 AXA 的子集 R 且符合

1. 自反性: $$ \forall a \in A $$, 必有 $$ (a, a) \in R $$
2. 反对称性: $$ (a, b) \in  R $$  且 $$ (b, a) \in R $$，当且仅当 a = b
3. 传递性

等价关系: 同偏序关系，但第2条为对称性: $$ (a, b) \in R \Rightarrow (b, a) \in R $$

定义 $$ [a] = \{ x \in A \mid x \sim a \} $$, 等价关系相关定理：

$$ [a] \cap [b] \neq \varnothing \Rightarrow a \sim b, and \, [a] = [b] $$

$$ [a] = [b] \Leftrightarrow a \sim b $$

即，多个等价类可以对一个集合A进行分割

商集：有等价关系～的类A，A/～ 定义为商集，由等价类组成集合，即 $$ \{[a] \mid a \in A \} $$

#### 1.4 映射

* 映射 是 A X B 的子集 f，对任意 a，仅有一个 (a, b) 属于 f
* A 为定义域，B 为培域，B 中一部分为值域

* 单射
* 满射
* 双射（等势、一一对应）

定义：

给定 $$ f: A \rightarrow B $$

$$ \exists f^{-1}: 2^B \rightarrow 2^A $$ 使对于一个 $$ D \in 2^B $$ 有 $$ D \mapsto f^{-1}(D) = \{ x \in A \mid f(x) \in D \} $$,

称 $$ f^{-1} $$ 为 f 的一个 **拉回(pull-back)** 或者 **逆(inverse)**, $$f^{-1}(D)$$ 为 D 上的**纤维(fiber)**.

同理可以定义一个推出：

$$ f:2^A \rightarrow 2^B, \, C \mapsto f(C) = \{ y \in B \mid y = f(x),\, x \in C\} $$

f 为一个推出，f(C) 为像.

卡式幂集: $$ B^A $$ 为从 A 到 B 的所有映射组成的集合。它与幂集存在一一对应(proof?)

Cantor-Bernstein-Schroder 定理: 如果集合 A 与 B 之间存在单射 $$ f : A \rightarrow B $$ 和 单射 $$ g: B \rightarrow A $$, 则它们之间存在一一对应(proof?)

#### 定义良好

关于映射定义，需要检验是否真是一个映射（不可以有一个元素映射到多个元素）

### 2. 群

定义群：群是一个集合 G 及其上的一个二元运算 $$ *: G \times G \rightarrow G $$, 并且满足：

* 结合律
* 有恒等元素
* 有逆元

若此群还满足交换律，则称为 Abel 群.

定理:

* 恒等元素只有一个
* 逆元只有一个
* $$ \forall a, b \in G, \, (ab)^{-1} = b^{-1}a^{-1} $$
* 任意元素逆元的逆元是它本身


