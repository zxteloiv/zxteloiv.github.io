---
layout: post
title:  "A Note for Software Design of A Neural Network"
date:   2016-05-22 12:00:00 +0800
tags: optimization machine_learning algorithm
categories: note learning
math_support: mathjax
---

Usually we build networks using many existing frameworks. But at the beginning of learning we may want to build one from scratch.

One of the key points in neural networks is the back propagation algorithm, and as I explained [before](http://libzx.so/note/learning/2016/05/12/yet-another-tutorial-for-back-propagation.html), the algorithm can be seen as another dynamic programming. We can benefit from this when writing codes.

Example codes are writing in Julia.

## class design

As a network keeps consistency of lots of internal data, like weights, it's better to take them as an object. Thus there will be a *Layer* class and a *Network* class.

Specifically, we have a *perceptron layer* and a *stack network* class, because we are building a simple feedforward neural network.

## layers

To take a layer as a independent unit, we may have to keep the inputs and outputs for each layer, even though they can be merged since the previous layers' outputs will be the inputs of the next layer. This can be useful if computations of partial derivative needs the inputs and outputs.

If we are implementing a batch update, we need to save temporarily the updates for all weights before we actually update any of them.

## network

A stack net is a composition of layers, thus the *forward*, *backward* and *batch_update* are just looping and calling appropriate functions of each layer, except that before *backwork* the network should compute the error of loss function w.r.t the outputs of the last layer, which is the duty of the network and layers are not aware of it.

## activation functions

Existing functions need to be broadcasted to the whole array as they are for a single number originally. And the partial derivative should be given, too.

## initialization

Randomly initialization of the weights is the common choice. But to prevent the activation function getting saturated, maybe the weights of edges point to the same output neuron should be normalized and sum to 1.


