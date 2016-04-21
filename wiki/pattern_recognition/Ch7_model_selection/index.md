---
layout: page
title: Ch7 model selection
math_support: mathjax
---


## Intro

- model: different type & variable parameters
- selection: find the model that gives the best performance
- evaluation: new test samples (good generalization or not)

ML: representation(hypothesis) + evaluation(loss/precision/F1/utility/...) + optimization(SGD/...)

generalization: every hypothesis must use some **assumptions** to generalize data

overfitting: blabla

curse of dimensionality: blabla

but in most applications data samples are not uniformly but centered at a low dimensional **manifold**

a manifold is

- a math structure (surface)
- locally but never globally measurable by euclidean (e.g. the surface of the earth)

## model selection principle

- No Free Lunch: no algo A better than B under any circumstances
- Ugly Ducking: no best feature set independent with problems
- Occam’s Razor

## evaluation for selection

- statistical test
- bias (higher gives worse prec) and variation (higher gives worse match) 
- ROC
- complexity of models

## resample

### Jackknife (Quenouille, 1949) (刀切)

remove one or more samples each time, use the remaining sample to estimate parameters. (never put back)

average the several estimation to get the final estimation. (hold-one)

(using robustness analysis, it can be proved that this method is better than a single average on the overall dataset)

### bootstrap (Bradley Efron, 1979) (自助)

each time select n samples from the dataset (put them back next time)

do estimation and average then.

### Holdout (留出)

one portion for training and one portion for testing

### cross-validation

10-fold

divide the samples into 10 fold, train on 9 portions and test on 1 part.

(???) (see wikipedia)

## ensemble

condition:

- a classifier is better than randomly guess
- a classifier is much more different that others


ensemble methods:

- different types of base classifiers
  - meta classifier
- same type of base classifiers

### stacked generalization

blabla

### bagging

use bootstrapping and votes on class tag

### random subspace (attribute bagging)

bootstrapping on features rather than data samples

base classifiers are often linear classifier and support vector machines

### adaboost

data sample weight, base classifier weight

(统计学习方法.李航.清华大学出版社.2012)

























