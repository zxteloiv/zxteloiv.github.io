---
layout: post
title:  "遗传算法复习笔记"
date:   2015-11-17 18:02
tags: learning genetic_algorithm 
categories: note learning
math_support: mathjax
---

### 1. 样例

1. 编码
2. 初始种群生成
3. 适应度评测
4. 选择（基于选择概率）
5. 交叉（随机配对（涉及交叉概率）、随机确定交换位置）
6. 变异

### 2. 约束条件处理

依实际问题选择，常用有三种. 例如函数

$$f(x) = x^2, \forall x\in \mathbb N^+ , x \in [0, 31]$$

#### 2.1. 搜索空间限定法

限制算法搜索空间，让搜索空间与可行解空间一一对应

对简单条件：a < x < b，直接调整编码，使得搜索空间被限制到约束内:

* 初始化个体的编码
* 交叉、变异个体的编码

#### 2.2. 罚函数法

优化目标中加入惩罚项(基于约束 h)

$$
\text{min }f(\mathbf x) + \delta\sum_i\phi_i(h_i(\mathbf x)) \\
$$

也可以在惩罚项中考虑迭代时间，初期惩罚小，后期惩罚大

#### 2.3. 可行解变换法

找出一基因型到表现型的变换，使其满足约束条件。

### 3. 流程

#### 3.1. 初始化

选择长度为 L 的 N 个串。

问题空间编码到 GA 空间。

编码原则：

* 与问题相关、低阶、短长
* 最小编码字符集

编码评估：

* 完备
* 健全（满射）
* 非冗余（一一对应）

常用编码：**二进制**

* 用二进制表示解空间，浮点数则为定长区间
* 可使用精度公式方便找到合适的精度：$$\delta = \frac{M - m}{2^l - 1}$$
* 解码公式：$$x = m + (\sum_ib_i2^{i-1})\frac{M-m}{2^l-1}$$

#### 3.2. 适应度函数 (fitness)

##### 目标函数映射成适应度函数

* 直接转换：$$Fit(f(x)) = \pm f(x) $$
* 界限构造：
  * 最小化问题，函数值越小适应度越大
  * e.g. 1 $$Fit(f(x)) = \left\{\begin{matrix} c_{max} - f(x), & f(x) < c_{max} \\ 0, & others \end{matrix}\right.$$
  * e.g. 2 $$Fit(f(x)) = \frac{1}{1 + c + f(x)} $$
  * 上面的常数可依估计选取

##### 适应度尺度变换

有时收敛速度变慢，重新选择。

- 线性变换：$$F' = aF + B$$
- 幂变换：$$f' = f^k$$
- 指数变换：$$f' = e^{-af}$$

#### 3.3. 算法参数

预设参数：

- 群体大小
- 终止迭代次数
- 染色体长度
- 交叉概率
- 变异概率

just do cross validation

#### 3.4. 遗传算子

##### 选择：

常用轮盘赌(Monte Carlo Selection)：选择概率与适应度占比成比例

##### 交叉：

随机选择一点，右边位置全部交换

交叉概率一般 0.25 ~ 0.75

亦可以选择两点，交换中间部分；

或者按屏蔽字交叉

##### 变异：

变异概率取较小： 0.01 ~ 0.2

随机改变某一位的值

#### 3.5 终止条件

- 当所有个体均驱向同一种类
- 迭代次数达到

终止时一般较接近，考虑选用最佳个体即可

### 4. 遗传算法的改进

- 算法的数学基础:包括算法的收敛性理论,早熟现象与欺骗问题,交叉算子的数学意义与统计解释,参数设置对算法的影响等。
- 算法与其他优化技术的比较和融合:充分利用遗传算法的大范围群体搜索性能,与快速收敛的局部优化方法混合产生有 效的全局优化方法,从根本上高遗传算法计算性能。
- 算法的改进和深化:据实际应用不断改进和完善算法的编码策略、基因操作方法、参数选择等。
- 算法的并行化研究:并行遗传算法(PGA)正成为一个重要的研究方向。
