---
layout: page
title: Neural Semantic Parsing Tutorial (ACL 2018)
math_support: mathjax
---


Semantic Parsing, basically consists of two target
- QA (for the sake of returned value)
- Instruction (for the sake of side-effects)

## 1. Datasets

There're 4 aspects of research
1. for executable representations
2. understanding in a situated environment
3. generalizing to broader domains
4. discourse or long-term language understanding

### Executable Repr.

GeoQuery, Jobs (< 1K examples)

ATIS (27 tables and 160K entries, 5K examples)

CoNaLa: NL pseudocode to python (2500 curated examples, 600K noisy examples)

Django, HearthStone and Magic the Gathering, NL2Bash, CONCODE

WikiSQL ([Zhong, Xiong, and Socher, 2017](https://arxiv.org/abs/1709.00103))

### In A Situated Env.

SAIL: VR 3D
NLVR: images

Referring expressing: 
Blocks world: move blocks around
Room2Room: navigation in a realistic home

### Broader domains

AMRBank
WikiTableQuestions
Free917, WebQuestions

### Sequential Language Understanding

ATIS-Interactions

SCONE

SQA ([Iyyer et al. 2016](https://arxiv.org/abs/1611.01242))

Task-oriented dialogues over knowledge bases ([Eric et al. 2017](https://aclanthology.coli.uni-saarland.de/papers/W17-5506/w17-5506))

## 2. Constrained Decoding

neural models usually output syntactically or semantically errors

**Token-based decoding:**

Seq2Tree (Dong and Lapata, 2016): syntactically correct, but semantically error
Sketch-Constrained Seq2Tree (Dong and Lapata, 2018)

**Grammar-based decoding:**
[Krishnamurthy et al. 2017](https://aclanthology.coli.uni-saarland.de/papers/D17-1160/d17-1160)
Neural Semantic Parsing with Type Constraints for Semi-Structured Tables

## 3. Training

Fully supervised (with LFs) v.s. weakly supervised

- Maximum Marginal Likelihood method
- Structured Learning methods
- Reinforcement Learning methods
- Hybrid ones...

### MML

Given $D = \{x_i, w_i, y_i\}_{i=1}^N$, where w is the world data, y is the correct answer.

Goal: $$
\begin{align}
\max_\theta \prod_{x_i, y_i \in D}p(y_i\mid x_i; \theta)
  &= \max_\theta \prod_{x_i, y_i, w_i \in D} \sum_z p(y_i, z \mid x_i; \theta) \\
  &= \max_\theta \prod_{x_i, y_i, w_i \in D} \sum_z p(y_i\mid z)p(z \mid x_i; \theta)
\end{align}
$$

for approximating Y:

heuristic search (usually bounded, by length or something else)

- online search: during training, candidates varying but less efficient
- offline search: consistent LFs are searched before training

### Structured Learning

common in traditional semantic parsers (margin based or latent variable perceptron)

typically involve heuristic search over the state space

unlike MML, arbitrary cost function is available

training typically maximized margins or minimizes expected risks

### RL method

with MML:

- logic forms is also approximated, same with MML
- but approximation is done using sampling

comparison with SL:

- like SL, reward function is arbitrary
- unlike SL, reward is directly maximized

using REINFORCE (policy gradient)

### Bridging together

MML + RL: [Guu et al. 2017](https://aclanthology.coli.uni-saarland.de/papers/P17-1097/p17-1097)

RL + SL: [Iyyer et al. 2017](https://aclanthology.coli.uni-saarland.de/papers/P17-1167/p17-1167)

## 4. SP as Code Generation

- single line
- method level
- class level

## 5. context-dependent

context consists of

- previous instructions
- previous interpretations
- current world state
- previous world state

[Suhr et al. 2018](https://aclanthology.coli.uni-saarland.de/papers/N18-1203/n18-1203)




