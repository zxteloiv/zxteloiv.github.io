---
layout: page
title: QA with Graph Embedding 2014 2015
math_support: mathjax
---


## Bordes et al. 2014b

Open Question Answering with Weakly Supervised Embedding Models

### tags

ECML PKDD 2014, ReVerb, paraphrase, multitask learning, WikiAnswers

### Intro

question -> vector embedding
triples  -> vector embedding

then (simple factual) questions can be answered by comparing vector similarities

$$
\hat t(q) = \arg\max_{t'\in K} S(q, t')
$$

merit:

- no grammars predefined
- usable on any KB schema
- without the needs to define an intermediate LF

### question generation

1. pick a triple randomly
2. generate randomly one of seed questions

![QQ20161014-0@2x.png](resources/79D2FB9F06932C4623B6D231EEA01339.png)

weak training signal and noisy data.

- syntactically simple
- may be semantically wrong

### embedding 

scoring function

$$
\begin{align}
S(q, t) &= f(q)^T g(t) \\
f(q)    &= V^T \phi(q) \\
g(t)    &= W^T \psi(t) \\
\end{align}
$$

where _f_ and _g_ are embedding vectors, and _phi_ and _psi_ are binary representation of questions (bag of words)
and triples

$$
V \in R^{n_v \times k} \\
W \in R^{n_e \times k}
$$

### training

generated (weak) supervised training set:

$$
D = \{(q_i, t_i), i = 1, ..., \vert D\vert\}
$$

1. sample a pair $$(q_i, t_i)$$ from D
2. create a negative pair $$t'_i, \text{ such that }t'_i\ne t_i$$
3. SGD update to $$\min\, [0.1 - f(q_i)^Tg(t_i) + f(q_i)^Tg(t'_i)]_+$$
4. normalize embedding vectors

multitask learning: for the paraphrase task, to optimize

$$
S_{prp}(q_1, q_2) = f(q_1)^Tf(q_2)
$$

optimize the **both**. (alternating training steps)

### fine tuning

when SGD stops, many correct answers are near the top but not at the first place.

since the only use of embedding is to do dot-product, add another dot matrix:

$$
S_ft(q,t) = f(q)^TMg(t)
$$

and loss function is changed as:

$$
\min_M\frac{\lambda}{2}\Vert M\Vert_F^2 + \frac{1}{m}\sum_{i=1}^m[1 - S_{ft}(q_i, t_i) + S_ft(q_i, t_i')]^2_+
$$

## Bordes et al. 2014a

Question Answering with Subgraph Embeddings

### tags

EMNLP-2014, WebQuestions

- encoding not only a triple but also a subgraph
- inference for longer path

**training and loss function is similar above**, and so is the paraphrase **multitask training**

### one-hot vector representation

to train an embedding matrix, there kinds of one-hot representation are tried:

- single entity: only the answer entity is set 1 in the one-hot vector
- path representation: answer is encoded as path the question entity to answer entity, (triple or quadruplet)
- subgraph representation: all path, and all entity and connected entities are included

### inference

$$
\hat a = \arg\max_{a'\in A(q)}S(q,a')
$$

to avoid enumerate all triples, choose a candidate answer set first. either one:

- C1: all direct connected entities
- C2: all 1-hop triples (1.5x score) and beam search top 10 relation type (and thus corresponding entities)

for the question with multiple answers, the binary representation of these entities are averaged.

## Bordes et al. 2015

Large-scale Simple Question Answering with Memory Networks

### tags

SimpleQuestions

old system problem:

- how do existing systems perform outside the existing question template
- whether model trained on a dataset transfers well on other datasets
- whether such systems can learn from different training sources (_to capture all questions_)
- Reasoning depends on KB structure, but deliberatly curated KB like freebase can answer much more questions

### MemNN

#### Input module:

- multiple objects for the same subject and relation are merged into a set
- mediator nodes are removed
- facts in binary vector, similar for ReVerb facts but processed with Generalization module

#### Generalization module:

link ReVerb to freebase:

- use (Lin et al. 2012)
- at least one alias of freebase entity matched with Reverb entity string

otherwise, use bag of words for Reverb facts

#### Output module:

1. generate a candidate set: question -> freebase entities -> filter some -> find two with the most links
2. scoring using cosine distance for two embedding vectors

#### Response module:

just returns the set of objects of the selecting supporting fact

### train

dataset:

- SimpleQuestions: question, triple fact
- WebQuestions(distant supervision): question, answer entity alias
- automatic question generated from FB: question, triple
- paraphrase (multitask training)

loss function is similar







