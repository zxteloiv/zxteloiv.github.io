---
layout: page
title: Ch6 artificial neural network
math_support: mathjax
---


## history

blabla

## model

blabla

## perceptron learning

blabla

## multi-layer perceptron

blabla

误差梯度 = 权重边指向节点的误差信号 * 起始节点的输出

$$\Delta\omega_{hj}=\eta\delta_j y_h$$

### BP discussion

activation func is:

- non-linear and differentiable
- bounded
- better to be monotonic

data is better to

- be scaled to zero mean
- have equal variance of features (but not necessary for big data since it has already the relatively full information)
- use gaussian noise / prior expert knowledge for augmentation

use class number for nodes in the output layer

init weight matrix randomly

regularization to prevent overfitting

**adaptive learning rate**

add momentum

more references for optimization:
https://www.cs.toronto.edu/~hinton/csc2515/notes/lec6tutorial.pdf
http://sebastianruder.com/optimizing-gradient-descent/

functions:

- square error
- cross entropy
- Minkowski error

regression: square error
classification: softmax (??)

### BP problems

- gradient vanish / explode
- local optima
- network paralysis, which may be solved by:
  - using activation function that doesn't saturated
  - normalize weight periodically 


## feedback NN

hopfield: a node give output to all other nodes except itself

the relation between hopfield & pagerank & $$\pi$$?

## deep learning

history .... blablabla ....
network structure .... blablabla ....

- loss func may be local optimum
- train each layer by layer (unsupervised, greedy) (auto-encoder pre-train)
- go deeper (without auto-encoder pre-train, no deep learning or training??)

## CNN

- shared weights
- local connectivity

one layer weights saved:

suppose the image is 200\*200, and next hidden layer has 196 \* 196 nodes

| methods                 | weights amount |
| ----------------------- | -------------- |
| raw                     | (200 \* 200) \* (196 \* 196) |
| shared weights only     | (200 \* 200) \* (1) |
| local connectivity only | (5 \* 5) \* (196 \* 196) |
| both                    | (5 \* 5) \* (1) |

then the hidden layer has 196 \* 196 outputs, use pooling

## auto-encoder

x -> encoder -> decoder -> z,  $\min \sum_k \parallel x_k - z_k \parallel^2$

may iterate more and more times: z -> x -> z -> x -> z -> ...















