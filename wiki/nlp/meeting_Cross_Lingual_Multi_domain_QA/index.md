---
layout: page
title: meeting Cross Lingual Multi domain QA
math_support: mathjax
---


QA: 

1\. Do you define embedding for relation?

word embedding is for corpus, (h, r, t) is for relation in KB

2\. explain the loss function

each KB has constraints such that h + r = t
different KBs have constraints such that the aligned concepts have the same embedding

3\. how to get the seed words for different KB

human annotation, string match

4\. is this the only method or the best among different candidates?

the best, using structual matching rather than string matching

5\. why could it surpass other methods?

use context when string matching failed

6\. what is attention-based neural network?

add weights to the focused words in question

7\. which method is popular?

Semantic Parsing is for KB and Retrieval based QA is for web corpus in tradition ...

8\. motivation




