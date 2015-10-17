---
layout: post
title:  "Notes on neural machine translation system"
date:   2015-10-17 16:36
tags: learning machine_learning neural_network algorithm machine_translation
categories: note learning
math_support: katex
---

This blog is inspired by [an NVIDIA tutorial](http://devblogs.nvidia.com/parallelforall/introduction-neural-machine-translation-with-gpus/) on building a machine translation system using neural networks.

I wanna write out my own thought after reading this post.

## Thoughts of the NVIDIA post

People have built a lot of MT(machine translation) systems using the statistical way, and we call them the Statisticall Machine Translation (SMT) systems.

### SMT with Training Data

We can obtain dataset from the Internet for MT, such as from [Workshop on Statistical Machine Translation](http://www.statmt.org/wmt15/) or [the International Workshop on Spoken Language Translation](https://sites.google.com/site/iwsltevaluation2014/home).

Using the data in pairs of $$ D = \{(x^1, y^1), \cdots , (x^N, y^N)\} $$ where x stands for the source sentence and y the target sentence, we can build an SMT by maximizing the log-likelihood for the dataset.

SMT may adopt neural networks as feature functions or some reranking strategies. But this post focuses on building an MT in a COMPLETE neural network way.

### Recurrent Neural Network

To output something $$ y_i $$, a recurrent neural network may use some affine transformation like $$ h_t = \tanh(Wx_t + Uh_{t-1} + b) $$.

To implement this kind of RNN is easy in theano. See [RNN in materials](#RNN).

Other sophisticated activation functions may be useful: 

* long short-term memory units[Hochreiter and Schmidhuber, 1997]
* gated recurrent units[Cho et al., 2014]

These are sophisticated but easy to implement in theano. See [LSTM in materials](#LSTM).

In RNN, we model a sentence by $$ p(x_1, x_2, ..., x_T) = \prod_i p(x_i \vert x_{i-1},...,x_1) $$.
And we use $$g_\theta$$ to produce a probability, $$h_{t-1}$$ is the previous hidden state.
$$
p(x_i \vert x_{i-1},...,x_1) = g_\theta(h_{t-1})
h_{t-1} = \phi_\theta(x_{i-1}, h_{t-2})
$$

The author gives one of his slides to explain how to use [RNN for language modeling](#RNN).

### An Encoder-Decoder Architechture for MT

Encoder-Decoder Architechture is old, but here we use RNN's for encoder and decoder part.

Using RNN, the encoder reads in a sentence, and stores it as a representation at the last hidden state $$h_T$$. 
By using PCA to reduce dimensions to two, we can see how the last representation of a sentence is like.

Encoder flow is: input words (1-hot) -> continuous representation -> RNN hidden states

Decoder flow is: RNN states -> word probability -> word sample.

To train a model like this only needs tradition methods.

* Back-prop
* SGD
* MLE

Gradient computation is easy with [Theano](#theano).

Adjusting parameters is frustrating. So we have adaptive learning rate [algorithms](#adaptive).

### Introduce Attention-based Mechanism

The big problem in Encoder-Decoder architecture: the performance decline while the sentence gets longer.

We may use two RNN's. One reads the sentence from left and the other from right.

**NOT QUITE UNDERSTAND THEN**  
**TODO: DO MORE INVESTIGATION**

And to include **a small neural network in a decoder** to do almost exactly **this**. This small network is called attention mechanism. It has a nice [performance](#attention).

## Other things to be investigated then

[deeplearning.net](http://deeplearning.net/tutorial/contents.html) materials:

<a name="RNN"></a> Recurrent Neural Network **with word embeddings**

* how to do inference?
* **how to train an RNN using back-prop?**
* try to write one in 10 lines in theano
* **RNN for language modeling**

<a name="LSTM"></a> LSTM **for sentimental analysis**

<a name="theano"></a> **Theano gradient** computation:

* code sample: https://github.com/lisa-lab/DeepLearningTutorials/blob/master/code/lstm.py#L457
* docs: http://deeplearning.net/software/theano/library/gradient.html

<a name="adaptive"></a> **Adaptive learning rate** algorithms:

* Adadelta [Zeiler, 2012]
* Adam [Kingma and Ba, 2015] 

<a name="attention"></a> Attention-based problem:

* What problem are we confronting?
* What is attention?
* Attention performance: [Bahdandau et al., 2015](http://arxiv.org/abs/1409.0473)

Other deep learning reference:

* UFLDL: http://ufldl.stanford.edu/tutorial/
* Neural Networks and Deep Learning online book: http://neuralnetworksanddeeplearning.com/index.html

