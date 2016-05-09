---
layout: page
title: Lecture Deep Sentence Modeling for Question Answering
math_support: mathjax
---


Zhiguo Wang. IBM Watson

Understanding and modeling the sentences.

## Abstract

Sentence modeling is a crucial procedure for question answering. In this presentation, I will talk about applying deep learning methods for sentence modeling, including sentence clustering, sentence matching, sentence classification and sequential labeling. I will also present how to apply these technologies into real world question answering systems.

## Overview

### factorid:

similarity / overlap: question & passages

architecture:

Q -> lucene search(wiki)
-> passage generator -> passage ranker(DL, sen-matching)
-> chunk extractor(DL, seq labeling)
-> feature generator(human)
-> chunk ranker(DL)

chunk: sentence contains the correct answer

### others

all of other non-factoid q, use FAQ-based

FAQ: classify ->  similarity -> ensemble

## sen-clusering

use companies' custem servies log, to build FAQ training set (with human intervering)

traditional: bag-of-words / TF-IDF, k-means  
drawback:

- BoW / TF-IDF intrinsic problems
- human knowledge to clustering better than pure unsupervised

so semi-supervised. let the clustering be like what we want

modeling: CNN (for text representation ??) / LSTM

loss func: (k-means term) + (max labeled margin term)

## sen-matching

motivation:

- lexically different representation for the same sentence
- not only word-word level but also phrase-level and syntax-level

model: blabla

## sen-classification

???

## seq labeling

chunk -> seq labeling -> answer begin, w, w

better than NER, avoiding the large ranking set


