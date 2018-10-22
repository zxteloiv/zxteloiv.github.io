---
layout: page
title: Prob. Graph 1
math_support: mathjax
---


Probabilistic Method and Random Graph

# Lecture 1 - Elementary prob theory with apps

## Prob. axioms 

Space

$$
\Omega \ne \emptyset \\
F \subseteq 2^\Omega \\
Pr: F \to R
$$

F is a $\sigma$-algebra

Pr:

$$
Range(Pr) \subseteq [0, 1] \\
Pr(\Omega) = 1 \\
Pr(\bigcup_{i \gt 1} E_i = \sum_{i\ge 1} Pr(E_i) \\
\text{countably many events are mutually disjoint}
$$

## Union Bound

inclusion-exclusion principle

**Union bound**

Bonferroni Inequalities

## Independence

Chain rule:

$$
Pr(\bigcap_{i=1}^n A_i) = \prod_{i=1}^n Pr(A_i \mid \cap_{j=1}^{i-1} A_j)
$$

> Independence is hard to prove

## Basic Laws

total probability:

If $E_1, E_2, ..., E_n$ are mutually disjoint and $\bigcup_{i=1}^nE_i = \Omega$, then $Pr(B) = \sum_{i=1}^n Pr(B\cap E_i) = \sum_{i=1}^nPr(B\mid E_i)Pr(E_i)$

bayes law

## Monty Hall problem

Formal proof: The Monty Hall Problem, Afra Zomorodian, 1998

## Random Variable

RV is a real-valued function on the sample space of a probability space, $X: \Omega \to R$.

### RV Properties as Probability:

Prob of an RV
- $X=a$ is the event $\{s\in \Omega \mid X(s) = a \}$.
- $Pr(X=a)=\sum_{s\in\Omega:X(s)=a}Pr(s)$

similar def. for independent RVs

### RV Properties as Numbers:

Expectation exists iff. Exp converges

**linearity of expectations**

## Distributions

Bernoulli: $X^k = X$

Example: how many triangles among 4 nodes when the links appear independently randomly?

Binomial RV

Example: Router packets sampling

Geometric Distribution

Example: the farmer and the rabbit

Memoryless: $Pr(X = n + k \mid X > k) = Pr(X = n)$ , only the Geometric Distribution satisfied (among discrete)


