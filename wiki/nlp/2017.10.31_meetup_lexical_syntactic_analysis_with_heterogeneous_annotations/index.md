---
layout: page
title: 2017.10.31 meetup lexical syntactic analysis with heterogeneous annotations
math_support: mathjax
---


methods 
- stacking
- multi-view learning
- pretraining

transition-based method:
![IMAGE](resources/478C4B9C45E11CA889101E3782A96964.jpg =678x303)

根据状态灵活提取特征，不用针对整个图提取特征

features:

uni-/bi-gram embedding + length-5-window

结构学习（而不是动作学习）更有用

beam search: 保留 gold action sequence 而不是优化最高得分

NMT：encoder 的作用较大，并不是一个结构 search 的难题

methods
- distillation
- multitask
  - adversarial
  
ZPar: http://people.sutd.edu.sg/~yue_zhang/doc/index.html

**A Neural Probabilistic Structured-Prediction Model for Transition-Based Dependency Parsing**
http://anthology.aclweb.org/P/P15/P15-1117.pdf
Neural probabilistic parsers are attractive for their capability of automatic feature combination and small data sizes.
A transition-based greedy neural parser has given better accuracies over its linear counterpart.  We propose a neural
probabilistic structured-prediction model for transition-based dependency parsing, which  integrates  search  and learning.
Beam search is used for decoding, and contrastive learning is performed for maximizing the sentence-level log-likelihood.
In standard Penn Treebank experiments, the structured neural parser achieves a 1.8% accuracy improvement upon a competitive greedy neural parser baseline, giving performance comparable to the best linear parser.

**Syntactic Processing Using the Generalized Perceptron and Beam Search**
http://www.mitpressjournals.org/doi/pdfplus/10.1162/coli_a_00037
http://www.mitpressjournals.org/doi/abs/10.1162/coli_a_00037

**Neural Word Segmentation with Rich Pretraining**
https://arxiv.org/pdf/1704.08960.pdf
https://arxiv.org/abs/1704.08960

**Neural Network for Heterogeneous Annotations**
http://www.aclweb.org/old_anthology/D/D16/D16-1070.pdf


