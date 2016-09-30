---
layout: page
title: MT Meetup
math_support: mathjax
---


# Translation

## Background

pass

## MT Techniques

### SMT

噪声信道：log-linear

baidu: phrase - 层次短语 - 句法 - NN

### KD

- bilingual webpage
- official site bilingual
- cross-transferable links in wikipedia

**webpage topology analysis**: webparse, region annotation, content block, coordinate distance

noise: ML, confidence, linguistic features, collect pos / neg samples

### practice

word alignment:

$$
P(F\mid E) = \sum_A P(F, A\mid E)
$$

phrase extraction (from word alignment)

phrase Trans: Chinese - seg - trans - permutation - English

decoding:

- Beam Search (Koehn 200?)
- Cube Growing (Liang Huang et al., 2007) (further pruning)

### highlights

**multipolicy: phrase / RNN / SMT / ...**

A: mainly NN, webpage (much text, phrase-based)

adaption with domain-dependent corpus

mobile / embedding: lexicon pruning (synonymy) / model compression / dynamic loading

**pivot language (?? pivot lang choice)**

A: English + random walk for noise reduction

- improve using random walk

domain

## Product Application

Web / APP / API

# Recent Advances in MT

Phrase-based SMT / Syntax-based SMT / Neural MT, (BLEU 21 : 22 : 24, 2016)

## NMT

seq2seq:

$$
P(y\mid x; \theta) = \prod_{n=1}^N P(y_n \mid x, y_{\lt n}; \theta)
$$

GRU for long sentence

## Recent Advances

### improving networks for NMT 

attention

multi-lingual / multi-task: mutli language may find better word sense representation than single-language (cross-reference)

??

character-level:

- A Character-level decoder without explicit segmentation for NMT (Chung et al. 2016)
- Achieving Open Vocabulary NMT with Hybrid Word-Character Models (Luong and Manning, 2016)

character ? 偏旁? some word is recognized as a whole while others can be understood in parts

### ideas from SMT

use features from SMT

## conclusion

pass

# LingoSail: Human-Machine Intelligent Translation

No specific feedback.

















