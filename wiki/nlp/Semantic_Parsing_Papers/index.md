---
layout: page
title: Semantic Parsing Papers
math_support: mathjax
---


## Zelle, Mooney, 1996

Learn to Parse Database Queries.

### tags

variable-free logic, shift-reduce parsing, inductive logic programming

### contents

Use database schema:

- basic objects: cityid, stateid, ...
- basic predicates: capital(), city(), major, ...
- meta predicate (high-order predicates): answer / largest / lowest / count ...

## Zelle, Mooney, 1993

Learning Semantic Grammars with Constructive Inductive Logic Programming

semantic grammar acquisition viewed as the learning of search control heuristics in a logic program

### tags

shift-reduce, 

### contents

![QQ20160908-0@2x.png](resources/6484A607496A2E429DEA709BFA3C1A4D.png)

#### flaw of connectionist models

1. output structure is flat, thus it's unclear how embedded propositions can be handled
2. give only a single output structure even if the sentence is truly ambiguous

#### CHILL (constructive heuristics induction for language learning)

1. introduce an overly-general parser using training data (may give spurious analyses)
2. specialize the parser by search control heuristics

CHILL Induction

![QQ20160908-2@2x.png](resources/0BA07F0491682954232BF3B6172AE5F9.png)

For each pair clause in sample:

- **Find_Generalization**: takes the pair, construct a new clause that subsumes them while not covering any negative example
- **Reduce_Definition**: uses new clause to prove positive examples. New clause is more prefered and any clause not used is deleted.

Until the generalized definition will not change.

## Zettlemoyer, Collins, 2005

Learning to Map Sentences to Logical Form: Structured Classification with Probabilistic Categorial Grammars

### tags

probabilistic CCG, log-linear model, sentence-LF pair,

(CKY, high-order logic)

### background

#### lambda calculus

lambda expressions have:

- constants: entity / number / function
- logical connectors
  - conjunction $\wedge$
  - disjunction $\vee$
  - negation $\neg$
  - implication $\to$
- quantification: $\forall, \exists$
- lambda-expressions $\lambda x.borders(x, texas)$
- additional quantifiers $count, \arg\max, \arg\min, \iota$
  - a definite operator would return the unique item for which state(x) is true, if a unique item exists
  

#### CCG

Combinatory Category Grammars (Steedman, 1996, 2000)

- lexicon: word to category
- category: (S\NP)/NP
- composition rules:
  - functional application (left / right)
  - type raising


### parsing

#### parsing uses PCCG

ambiguity comes from:

- lexical item with multiple entry in lexicon
- spurious ambiguity: same semantic but different derivation

log-linear model: (L=logical form, T=derivation sequence, S=sentence)

$$
P(L, T \mid S; \bar\theta) = \frac
  {\exp(\bar f(L,T,S)\cdot\bar\theta)}
  {\sum_{(L,T)}\exp(\bar f(L,T,S)\cdot\bar\theta)}
$$

and parsing involves PCCG inference:

$$
L = \arg\max_L P(L\mid S;\bar\theta) = \arg\max_L\sum_TP(L,T\mid S;\bar\theta)
$$

use dynamic programming(beam-search actually): features are only **local** lexical feature (number of occurence of a lexical entry in T)

#### estimation of theta

T is latent. maximize the log-likelihood.

$$
\begin{align}
O(\bar\theta)
  &= \sum_i\log P(L_i\mid S_i; \bar\theta) \\
  &= \sum_I\log (\sum_T P(L_i, T\mid S_i; \bar\theta))
\end{align}
$$

differentiate it w.r.t theta and then use dynamic programming (variant of inside-outside algorithm) 

### induce the lexicon

#### GENLEX

GENLEX generates lots of lexical entries of which some may be pruned later.

$$
GENLEX(S,L) = \{x := y \mid x \in W(S), y\in C(L)\}
$$

- W(S) is all subsequence of S
- C(L) maps L to a set of categories using rules which will produce categories when triggered by a constituent of logical forms.

![QQ20160910-0@2x.png](resources/B58945F8CA44CE38DC6F210EAFCE921B.png)

all possible lexicals:

$$
\Lambda^* = \Lambda_0 \cup \bigcup_{i=1}^n GENLEX(S_i, L_i)
$$

Weights are set to 0.1 for entries in Lambda_0 and 0.01 for others.

#### learning

basically two steps:

![QQ20160910-1@2x.png](resources/CEC6AD08D0189347857AD046E2590816.png)

- step1 produce a lexicon Lambda\_t compacter than Lambda^star
- step2 keeps the log-likelihood be optimized for Lambda_t

PARSE function will parse the tree and gives the highest probabilities.

### problems

GENLEX is controlled by rules, and will be insufficient if the rules don't cover all the sentence - logical forms. (lexicon and composition can both limited recall) e.g.

> Through which states does the Mississippi run.

GENLEX doesn't trigger a category suitable for the _through_-adjunct be placed ahead.




