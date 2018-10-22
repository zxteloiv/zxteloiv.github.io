---
layout: page
title: 2018.08.20 DL for Semantic Composition and NLI
math_support: mathjax
---


Abstract

Reasoning and inference are central to both human and artificial intelligence (AI). Modeling inference in natural language is notoriously challenging but is a basic problem towards true natural language understanding. In this talk, I will introduce the state-of-the-art deep learning models for natural language inference (NLI). The talk will also discuss a more fundamental problem: learning representation for semantics and composition. We will focus not only on how deep learning models achieve the state-of-the-art performance but also on their limitations.

## Intro

**Human-like AI [Lake, 2016]:**
Build machines thinking and learning like people
- Compositionality
- **Causality models/reasoning/inference**
- Commonsense
- Learning-to-learn

Potential Solutions

- convert to LFs (hard and lossy)
- Data-driven (definitely limited)

## Cross-sent-att based

ESIM and Tree-based [Chen et al., ACL2017]

## Sent-embed based

no cross-sentence attention here.

**semantic composition**

refer to tutorial at ACL 2017

RepEval model

## Topics

NLI with external knowledge ... [Chen et al. ACL 2018]

> external knowledge may not be learnt from training data.
> purely from training data, copy-mechanism might be insufficient

new inference dataset [Glockner et al. 2018]

Generalized pooling (??) for NLI


