---
layout: post
title:  "Language models revisited"
date:   2015-10-21 21:54
tags: learning natural_language_processing language_model
categories: note learning
math_support: katex
---

This blog is inspired by an excellent slides *From Sequence Modeling to Translation* by Kyunghyun Cho. You can view it [here](https://drive.google.com/file/d/0B16RwCMQqrtdNEhwbHN2bXJzdXM/view).

## Why language models

When it comes to the NLP problem, we often want to know how **likely** a sentence is, that is, for a sentence $$ s=w_1w_2...w_T $$, we want to model the probability:
$$
P(s) = P(w_1, w_2, ..., w_T)
$$

As we know, n-gram is the one of the most popular language models. n is typically 1, 2, 3 or even 4 for most of the time.

n-gram has made the following two assumptions:

1. Markov independence: the current word depends on previous n words.
2. Non-parametric Estimator: probability is directly counted.

namely, we get 

$$
P(s) = \prod_i P(w_i \vert w_{i-1}, ..., w_{i-n})
$$

where each probability on the right is computed by counting the occurrence of words in our training corpus, like

$$
P(w_i \vert w_{i-1}) = \frac{count(w_{i-1} w_i)}{\sum_j count(w_{i-1} w_j)}
$$.

n-gram models are facing two potential problems:

1. Rare terms: It's clear that our corpus don't cover all the word that may appear in the reality, which leads to some zero probabilities in n-gram while it's actually very possible.
2. No generalization: You know one word *w* is followed by another, but you cannot get the similar probability for other words close to *w* in their meaning.

To solve the first problem, people invented many smoothing methods. The word *smoothing* vividly explains what it does: to *smooth* the distribution for all n-grams, those with high probability are more or less lowered and the *spared probability* is then distributed to the n-grams with 0 probability.

There're quite a lot of smoothing methods and people have done many comparisions for them.

* Laplace smoothing or additive smoothing
* Good-Turing
* Katz smoothing
* Jelinek-Mercer smoothing
* Witten-Bell smoothing
* Kneser-Ney smoothing
* Church-Gale

## Move to Neural Network Language Models(NNLM)

### Relax the 2nd assumption
When we convert the non-parametric estimator to a parametric one, we disgard the previous n-gram probability and turn to a function parameterized by, say, theta.

$$
P(x_t \vert x_{t-n}, ..., x_{t-1}) \approx f_\theta(x_{t-n}, ..., x_{t-1})
$$

We denote the word embedding matrix with **W**, internal map with **U** and map with **V** back to vocabulary.

$$ \forall t, Wx_t $$ is the word's embedding.

Concatenate all the adjacent words together, we get the context vector.

$$
h_t = (x_t, x_{t-1}, ..., x_{t-n+1})
$$

The unnormalized probability is thus

$$
y = V(\phi(Uh+b)+c)
$$

Using softmax on it will yield the final probability we want.

$$
P(x_t=i\vert ...) = \frac{\exp(y_i)}{\sum_j\exp(y_j)}
$$

### Relax the first assumption

What if we do not preserve the n-th order markov assumption? We get

$$
P(x_1, ..., x_T) \approx \prod_{t=1}^T P(x_t \vert x_{t-1}, ..., x_1)
$$

We thus use the recurrent network to produce it. Every probability can be computed by 

$$
h_t = h_{t-1} \oplus x_t \text{ and } y_t = \triangleright h_t
$$

Here two operators are based on data and network design.

* Addition
* Pop Out

The addition may be implemented as some affine transformation(with a nonlinear function), and the Pop may be another affine one. The final probability can also be optained using softmax.

To train such an RNN, use Back-Propagation Through Time(BPTT). I haven't did any research on it now.

## Tips for RNN

RNN does have new problems, too.
* **Gradient Vanishing** may occur when the train is long.
* There's also so-called **Gradient Exploding**, too.
* RNN may not remember things with too long distance.

For the third problem, Gated Recurrent Unit(GRU) or LSTM may help. We can also choose to use **Attention-based Models**, but that's another question and needs a new post.

We may also adopt some **adaptive learning rate algorithm** (Adagrad, Adadelta, Adam, et al.) when training networks.

Theano gives the automatic differentiation, too.

## Bibliography

* Kyunghyun Cho. From Sequence Modeling to Translation. 2015. https://drive.google.com/file/d/0B16RwCMQqrtdNEhwbHN2bXJzdXM/view
* 宗成庆. 统计自然语言处理（第2版）. 清华大学出版社, 2013. Douban. Web. 21 Oct. 2015.

