---
layout: post
title:  "Attention Mechanism Quick Notes"
date:   2016-03-04 15:00
tags: machine_learning attention deep_learning end-to-end
categories: main learning
math_support: mathjax
---

Notes an attention-based methods.

## Intro

**Attention** is a word borrowed from psychology.

Here attention means to selectively concentrate on a small portion of the overall data. While the ideas are the same, the mathematical forms of this kind could be quite different.

Here the sequence-to-sequence task (usually RNN is used) is discussed more than others. In traditional RNN, one tries to encode the all input items into one final code. Then decode the code into mulitple output items. The problems for this method are ignored here.  
But if we adopt attention mechanism, we may selectively choose some input items, encode and decode it for every output step. Therefore we may only look at some of the input rather than a full and complete representation.

In general, there are four different types of attention, based on two different dimension.

- item-wise and soft attention
- item-wise and hard attention
- location-wise and hard attention
- location-wise and soft attention

The item-wise and location-wise attentions are different according to their different use cases. Item-wise attention can be used in tasks where each input items are given or can be extracted, while location-wise attention is for tasks where input items are hard to get and we therefore accept a feature map (like vision tasks) or make a transformation on that feature map to help the encoder utilize it in a later phase.

Since the attention module must choose some input (or input code as in item-wise way) to go further, the soft attention (usually) makes a linear combination for all the input, while the hard attention choose several of it in random (based on a distribution).
So the soft attention is an end-to-end network while the hard one is not. Thus the soft-attention could be learnt using gradient method but the hard one must use some learning method in reinforcement learning.

## Modelling

### basic neurons

A simple neuron, which could be seen as a stateless and reentrant function, may have the following form.

$$
o = f(\sum_{k=1}^n (\omega_k i_k) + \omega_0),
$$

where $$f$$ could be any non-linear activation function, i.e., sigmoid, ReLU.

A recurrent neuron has a more general form and there's no constraint of the function on what it should looks like.

$$
\begin{bmatrix}
\hat o_t \\
h_t
\end{bmatrix}
= \phi_W(i_t,h_{t-1})
$$

### models for sequence-to-sequence problem

without attention, encoder takes all of the input items.

$$
\bf{c} = \phi_{W_{enc}}(\bf{X})
$$

And decoder is another RNN.

$$
\begin{bmatrix}
\hat y_j \\
h_j
\end{bmatrix}
= \phi_{W_{dec}}(c, \hat y_{j-1}, h_{j-1})
$$

And the learning objective is just the log-likelihood.

$$
\begin{align}
L_j(X,Y,\theta) &= \log p(y_j \vert X, y_1, y_2, \dots, y_{j-1}, \theta)  \\
                &= \log p(y_j \vert \hat y_j, \theta)
\end{align},
$$

where $$\theta$$ is all the parameters like $$W_{enc}, W_{dec}$$

### attention model

#### item-based soft attention

For item-based soft attention method, every input item have a code.

$$
C = {c_1, c_2, \dots, c_T} \\
c = c^j = \phi_{W_{att}}(C, h_{j-1})
$$

We simply want the attention module to choose items using linear combination. Then the weights are computed using a softmax on some neural network $$f_{att}$$.

$$
e_{jt} = f_{att}(c_t, h_{j-1}) \\
\alpha_{jt} = \frac{\exp(e_{jt})}{\sum_{t=1}^T exp(e_{jt})}
$$

Then the final code at the current step $$j$$ is as following (linear combination):

$$
\begin{align}
c &= \phi_{W_{att}}(C, h_{j-1}) \\
  &= \mathbb{E}(c_t) \\
  &= \sum_{t=1}^T \alpha_{jt} c_t
\end{align}
$$.

#### item-based hard attention

Hard attention does not simply choose the items with the highest weight. It usually use a distribution to randomly draw from, making the items with higher weights are more likely but not definitly to be chosen. Thus, let the indices of the chosen items at the current step be $$l_j$$, then

$$
l_j \sim \mathcal{C} (T, \{\alpha_{jt}\}_{t=1}^T)
$$.

#### location-based attention

In location-based attention mechanism, since no input items are given and each time the input is a feature map (like an image), we do a transformation on it and send the final _glimpse_ to the encoder and get the code. I don't want to talk about it more here.

## Learning

For soft attention, just do normal gradient descent or something because the network is end-to-end and differentiable.

For hard attention, just do similar things in reinforcement learning and set a proper reword function then do Q-learning.

## Further Reading

1. Wang, F., & Tax, D. M. J. (2016). Survey on the attention based RNN model and its applications in computer vision. arXiv:1601.06823. Retrieved from http://arxiv.org/abs/1601.06823
2. A blog post. [attention and memory in deep learning and nlp](http://www.wildml.com/2016/01/attention-and-memory-in-deep-learning-and-nlp/)
3. [Show, Attend and Tell](http://arxiv.org/abs/1502.03044) for image labelling
4. [Grammar as a Foreign Language](http://arxiv.org/abs/1412.7449) for language parsing
5. [Teaching Machines to Read and Comprehend](http://arxiv.org/abs/1506.03340) for QA
6. [End-to-End Memory Networks](http://arxiv.org/abs/1503.08895) allow the network to read same input sequence multiple times before making an output, updating the memory contents at each step. 
7. [Neural Turing Machines](https://github.com/dennybritz/deeplearning-papernotes/blob/master/neural-turing-machines.md) use a similar form of memory mechanism, but with a more sophisticated type of addrdessing that using both content-based (like here) and location-based addressing, allowing the network to learn addressing pattern to execute simple computer programs, like sorting algorithms.
8. a clearer distinction between memory and attention mechanisms, perhaps along the lines of [Reinforcement Learning Neural Turing Machines](http://arxiv.org/abs/1505.00521), which try to learn access patterns to deal with external interfaces.


