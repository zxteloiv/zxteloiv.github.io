---
layout: post
title:  "A note for Back Propagation"
date:   2016-05-13 01:00:00 +0800
tags: optimization machine_learning algorithm
categories: note learning
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

$$W$$ is the variable. Our goal is to find the very $$W$$ that gives the smallest $$J$$ value.

And we will use the gradient descent algorithm, which needs to compute the gradient or partial derivatives for each parameter in $$W$$.




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

Suppose we are building a feedforward neural network that is very very deep (up to 100 layers), each layer consists several independent neurons eating the output from the previous layer.  Let neurons in two adjacent layers be fully connected (like a bipartite graph).

Denote the neurons in the first layer as $$O^{(k)}_i, i = 1, 2, \dots, n$$, and those in the second layer as $$O^{(k+1)}_j, j = 1, 2, \dots, m$$, and weight from $$O^{(k)}_i$$ to $$O^{(k+1)}_j$$ is denoted as $$w_{ji}$$ (**NOT** $$w_{ij}$$, the reason is below), that is,

$$
O^{(k+1)}_j = \sigma(\sum_i w_{ji} O^{(k)}_i)
$$

We can use matrix to rewrite the equation above for simplicity (that's why we use the notation $$w_{ji}$$, for omitting the transpose notation) :

$$
O^{(k+1)} = \sigma(W^{(k)} O^{(k)}), k = 1, 2, \dots, N - 1
$$

### computation

Here's where the BP algorithm actually comes in.
Let's take only one example to train, then the loss function is just $$ J(W) = 1/2 \parallel o - y \parallel^2 $$. Every $$o$$ is a constituent of the output of the last layer and thus they are unknown functions w.r.t. all parameter $$W$$.

The parameters $$W$$ is unknown, and we use gradient descent to find the local optimum of them, which means we have to compute the gradient or partial derivative for all $$W$$.

For the last layer, we could derive $$\partial J/\partial o = (o - y)$$.

Recall that: $$ O^{(k+1)} = \sigma(W O^{(k)}) $$, we could get all partial derivatives w.r.t. all parameters $$W$$ as follows (by intuition, the formula may be not formal):

$$
\frac{\partial J}{\partial W} = \frac{\partial J}{\partial O^{(k+1)}}\frac{\partial O^{(k+1)}}{\partial W} \\
\frac{\partial J}{\partial O^{(k)}} = \frac{\partial J}{\partial O^{(k+1)}}\frac{\partial O^{(k+1)}}{\partial O^{(k)}} \\
$$

So far, we've got the way to compute each gradient for every $$W$$ and also every $$O^{(k)}$$. The first is what we really want, and the second helps us to compute backward iteratively, i.e., use $$O^{(k+1)}$$ to get gradient for $$O^{(k)}$$ and then $$O^{(k-1)}$$ and so on so forth.

You may note that these $$O$$-s and $$W$$-s are all matrices, the derivative note is actually for matrices. If you try to compute each of them, some items could be ignored. For example, suppose $$w_ji$$ is the weight of from $$O^{(k)}_i$$ to $$O^{(k+1)}_j$$, when computing its gradient, all gradients of nodes in the $$(k+1)$$-layer except the $$j$$-th node are useless. They are not functions of $$w_ji$$ and thus their derivatives w.r.t. $$w_ji$$ are all ZERO.

Using words rather than formula, we know $$J$$ is function w.r.t $$O^{(k+1)}_j, j=1,2,\dots,d_{(k+1)}$$, and each $$O^{(k+1)}_j$$ is a function of all $$O^{(k)}_i, i = 1,2,\dots,d_{(k)}$$ and $$w_ji$$.

Just follow the rule to compute partial derivatives of compound functions above, we could get everything.

And note that this is somewhat similar to dynamic programming in computer science. We are breaking the whole problem into smaller ones, i.e. the partial derivatives between two adjacent layers. And we use the computed gradients of (k+1)-th layer directly, to compute the gradients of the k-th layer, which saves a lot of computations.

## 3. relation to history

People in various fields have re-invented the algorithms in many times. But they are just as simple as dynamic programming and partial derivatives actually, and can be derived directly using not too much knowledge of calculus.

Again, people keep saying about a **delta rule**. Actually the **delta** is the gradient(or error) of the (k+1)-th layer. They say we can use delta to multiply the error of this layer. That's pretty obvious, right? Delta is the partial derivative of higher order functions, and the rules for compound functions make us to use multiplication.

## 4. More References

I found this blog post pretty useful: http://sebastianruder.com/optimizing-gradient-descent/

If you want to know more about gradient descent and optimization, just follow it.

And the open course CS231n of Stanford provides some useful tips and notes on implementation of optimization too. http://cs231n.github.io/neural-networks-3/ You may like it.

