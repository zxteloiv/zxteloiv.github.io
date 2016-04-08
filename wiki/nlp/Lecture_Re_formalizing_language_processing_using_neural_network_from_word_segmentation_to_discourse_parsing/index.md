---
layout: page
title: Lecture Re formalizing language processing using neural network from word segmentation to discourse parsing
math_support: mathjax
---


Abstract: 最近，神经网络学习以及相关的连续向量空间表示渗透到了自然语言处理领域，传统的基于简单计数的统计学习以及人工组织的语言处理层次学习正在被类似的相关技术所重写。我们介绍最近我在这一方面的一组系统性的工作和结果，它包括从分词、句法分析乃至篇章分析的神经网络学习的工作。在最为底层的次嵌入方面，我们介绍我们引入的语义项嵌入的扩展和改进。我们枚举了在相关的机器翻译上的应用和改进。

## word seg

seq-anno drawback

- word score

### decoding

beam search

### training

### compare

Chen et al., 2015a, 2015b
Zhang et al., 2013
Sun et al., 2012, 2009

## probabilistic graph-based dep parsing with CNN

score: sub tree score sums to the entire tree

probabilistic models

ensemble of different order models for scoring:
- simple adding
- stacking a layer

## hierarchical discourse parsing

## semantic for MT

> embedding should be for meaning rather than words

a clique 
















