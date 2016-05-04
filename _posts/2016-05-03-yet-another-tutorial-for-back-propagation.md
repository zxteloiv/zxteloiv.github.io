---
layout: post
title:  "Yet another tutorial for Back Propagation"
date:   2016-05-03 12:00:00 +0800
tags: optimization machine_learning algorithm
categories: main learning
math_support: mathjax
---

Through my learning process on machine learning, back propgation was ever a confusing part. Many tutorials on the Internet focus on numeric calculation, cover the long history of the algorithm, or use examples and diagrams leading to a serveral-page-long paragraph. That's too complex for such a simple thing I think. If we can't explain the algorithm simply, we may not explain it very well and even make readers confused. So I decide to write it as simple as I can, and later I will write about its relation to history.

## 1. Prerequisites

Before we proceed, let's recall some basic concepts quickly.

### a neuron

A neural network is formed by many neurons. For clarity, let's omit the bias $$b$$ of a neuron and write it as follows:

$$
net = \sum_iw_ix_i \\
y = f(net)
$$

where $$net$$ means the _net summation_ over these $$x$$-s, $$f$$ is the activation function and we take sigmoid here $$f(z) = \sigma(z) = 1/(1 + \exp(-z))$$.

We can use many neurons to from a layer, and use many layers to build the whole network.

### loss function and optimzation

And the loss function is the target we want to minimize. Usually we may take the squared error:

$$
J(W) = \frac{1}{2}\sum_i \parallel o_i - y_i \parallel^2
$$

where $$o_i$$ is the output of the last layer, $$y_i$$ is the known label for a single sample, $$\parallel x \parallel$$ is the L2 norm of a vector.

$$W$$ is the variable. Our goal is to find the very $$W$$ that gives the smallest $$J$$ value. And we will use the gradient descent algorithm, which needs to compute the gradient or partial derivatives for each parameter in $$W$$.

### partial derivative for a compound function

Suppose we have many functions as follows:

$$
f = f(g_1, g_2, \dots, g_n) \\
g_i = g_i(h), i = 1, 2, \dots, n \\
h = h(x) 
$$

Then if we want to get the parital derivative of $$f$$ with respect to $$x$$, we may write:

$$
\frac{\partial f}{\partial x} = 
    \sum_i\frac{\partial f}{\partial g_i}\frac{\partial g_i}{\partial h}\frac{\partial h}{\partial x}
$$

## 2. How To: Back Propagation

### notations

Suppose we are building a feedforward neural network that is very very deep (up to 100 layers), each layer consists several independent neurons eating the output from the previous layer.

Let neurons in two adjacent layers be fully connected (like a bipartite graph).

Denote the neurons in the first layer as $$O^{(k)}_i, i = 1, 2, \dots, n$$, and those in the second layer as $$O^{(k+1)}_j, j = 1, 2, \dots, m$$, and weight from $$O^{(k)}_i$$ to $$O^{(k+1)}_j$$ is denoted as $$w_{ji}$$ (**NOT** $$w_{ij}$$, the reason is below), that is,

$$
O^{(k+1)}_j = \sigma(\sum_i w_{ji} O^{(k)}_i)
$$

Since every output and weight is a single number, we can use matrix to rewrite the equation above for simplicity (that's why we use the notation $$w_{ji}$$, for obeying the matrix multiplication rule and omit the transpose sign) :

$$
O^{(k+1)} = \sigma(W O^{(k)}), k = 1, 2, \dots, N - 1
$$

### computation

Let's take only one example to train, then the loss function is just $$ J(W) = \parallel o - y \parallel^2 $$

Every $$O^{(k)}$$ is a single

## 3. relation to history

## 4. Bibliography

