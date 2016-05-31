---
layout: page
title: Ch9 Support Vector Machine
math_support: mathjax
---


# course notes

## intro

find the best among those feasible models for linear-separable data

两类训练样本中离超平面最近的样本与超平面之间的距离是最大的

## linear separable, hard margin

1\. 分类正确，且没有尺度：

$$w^Tx + b \> 0 \to y(w^Tx + b) \ge c$$

方便起见 c 取 1

2\. 最优，距离最大：

$$\frac{\vert 1 - (-1) \vert}{\Vert w \Vert} = \frac{2}{\Vert w \Vert}$$

is equivalent to

$$
\min \frac{\Vert w \Vert}{2} \
s.t. \, y\_i(w^Tx\_i + b) - 1 \ge 0, i = 1, 2, \dots, N
$$

...solve...

## non-linear, soft margin

not linear separable or margin is too small

    experiment, C 2^-5 ... 2^15     1 .. .. ..   2 .. ..    ... ..     10      sum .. .. ..    

find the C that yields the smallest summed error

## non-linear SVM

map every sample to high dimensional space

the map function is hard to find but it's easier to find the dot product of the mapped sample

the kernel

$$
K(x\_i, x\_j) = \Phi(x\_i)^T\Phi(x\_j)
$$

the kernel matrix should be

* symmetry
* semi positive definite
* Mercer's condition

**multi-kernel learning**

## application

...blabla...

**multi-class problem**: Error-Correcting Output-Codes

# reading notes

## modeling

maximize margin

## Lagrange multiplier

why lagrange multiplier works

## problem solving and duality

dual problem transform

## KKT condition

...

## soft margin

add offset to margin

## kernel methods

final form contains dot product

## Mercey's condition

what makes a kernel kernel

## SMO

...


