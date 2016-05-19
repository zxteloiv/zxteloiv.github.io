---
layout: page
title: Lecture Ensemble Approaches to Class Imbalance Learning
math_support: mathjax
---


## 报告摘要（ABSTRACT）：

Many real-world classification problems have unbalanced classes, e.g., in fault detection and software defect prediction, where there are a large number of training examples for the normal class, but few for the abnormal classes. This talk gives an overview of some recent algorithms for dealing with class imbalance in machine learning, including ensemble approaches, sampling methods, evolutionary computation methods, and their combinations. First, we will discuss how diversity influences the classification performance, especially on the minority class, in ensemble classification algorithms. Then new ensemble algorithms are introduced and evaluated experimentally. Multi-class imbalance will be analysed and considered. The combination of ensemble learning and sampling techniques for dealing with class imbalance will be presented. Finally, we consider a new problem --- online class imbalance learning of data streams, where the majority and minority classes are not pre-defined and have to be learned and detected online. Some results are presented to demonstrate the effectiveness of the proposed algorithm.

## 报告人简介（BIOGRAPHY）：

Xin Yao is a Chair (Professor) of Computer Science and the Director of CERCIA (Centre of Excellence for Research in Computational Intelligence and Applications) at the University of Birmingham, UK. He is an IEEE Fellow and the Past President (2014-15) of IEEE Computational Intelligence Society (CIS). His work won the 2001 IEEE Donald G. Fink Prize Paper Award, 2010 IEEE Transactions on Evolutionary Computation Outstanding Paper Award, 2010 BT Gordon Radley Award for Best Author of Innovation (Finalist), 2011 IEEE Transactions on Neural Networks Outstanding Paper Award, and many other best paper awards. He won the prestigious Royal Society Wolfson Research Merit Award in 2012 and the 2013 IEEE CIS Evolutionary Computation Pioneer Award. He was the Editor-in-Chief (2003-08) of IEEE Transactions on Evolutionary Computation and is an Associate Editor or Editorial Member of more than ten other journals. His major research interests include evolutionary computation, ensemble learning, and their applications, especially in software engineering.

## introduction

motivation: real world problem is often imbalance

difficulty: poor generalization on minority class

objective:

- high performance on minority class 
- without bothering the majority class

### approaches

- re-sampling: over-sample minority / under-sample majority
- cost-sensitive
- classification ensembles (演化计算 (evolutionary) 中多用，"集体")

ensemble is an approach for divide-conquer, (new method will bother old accuracy ??)

ensemble is a weighted sum

$$
O = \sum_j w_jo_j
$$

- determine $$w_j$$
- determine training algorithms: bagging / boosting, negative correlation learning

## diversity in ensembles

diversity (still ongoing research): related to behavior rather than the name of classifier

- diversity is positive on the minority class
- positive to both AUC and G-mean

AdaBoost.NC (S. Wang, H. Chen and X. Yao, Negative correlation learning for classification ensembles. ...., 2010)

- random oversampling to rectify the imbalanced distribution
- encourage diversity

## multi-class imbalance learning

multiple (>2) uneven class distributions

- multi-minority or multi-majority
- can we do without class-decomposition (design new multi-class algorithms directly)

consider imbalance rate (avoid over-/under- sampling)

M.Lin, X. Yao, 2013 Apr, A Dynamic Sampling Approach to Training Neural Networks for Multi-class imbalance learning

## online class imbalance learning

data stream 

1. imbalance ?
2. contain minority ?
3. imbalance rate?

Online-Bagging + Re-sampling

## multi-objective class imbalance learning

- take single class performances as separate objectives
- multi-objective optimization algorithm (multi-objective evolutionary algorithms, MOEAs) as learning

easy to include an additional diversity term, a regularization term, etc.

H. Chen and X. Yao, "Multiobjective Nueral Network Ensemble...", 2010

## conclusion

- ensemble is useful
- diversity is the key issue
- online class imbalance learning
- theoretical analysis is needed




