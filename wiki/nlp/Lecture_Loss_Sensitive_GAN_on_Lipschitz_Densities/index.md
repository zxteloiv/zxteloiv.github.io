---
layout: page
title: Lecture Loss Sensitive GAN on Lipschitz Densities
math_support: mathjax
---


Lecture: Loss-Sensitive GAN on Lipschitz Densities

## Challenges

- GAN is unregularized model. (cannot match prior)
- requires infinity capacity

JSD problem

- unable to generalize
- vanishing gradient (a perfect discriminator, Arjovsky et al. 2017)

=> regularized GAN

## LS-GAN

...

Bounded -> derivative is less than kappa -> lipschitz densities

under some integral

### non-vanish gradient

non-vanishing gradient proval

non-parametric analysis (using LP to prove, despite the mode flation)

## Generalizability

GAN not ~ 

### model complexity

- ...
- ...
- bouned domain
- model size

bounded domain (value is bounded, not infty)

模型多项式复杂度 -> 有泛化性
指数复杂度 -> 无泛化性

## conditional LS-GAN

focus on classification problem: $L: X\times Y \to \mathbb{R}$, Y is label


