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

A neural network is formed by many neurons. Each of them could be written as: $$y = f(\sum_iw_ix_i + b)$$. For clarity, let's omit the bias $$b$$ and rewrite it as follows:

$$
net = \sum_iw_ix_i \\
y = f(net)
$$,

where $$net$$ means the _net summation_ over these $$x$$-s, $$f$$ is the activation function and we take sigmoid here $$f(z) = \sigma(z) = 1/(1 + \exp(-z))$$.

### loss function and optimzation



### partial derivative for a compound function

## 2. How To: Back Propagation

- formation

## 3. relation to history

## 4. Bibliography

