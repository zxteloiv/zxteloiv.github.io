---
layout: page
title: Lecture Discovering Keywords for Search, Covariate Shift, and Lifelong Learning
math_support: mathjax
---


### Abstract

by Bing Liu

In almost any application of social media content analysis, the user is interested in studying a particular topic. Collecting posts relevant to the topic from a social media data source is a necessary step. Due to the huge size of social media sources (e.g., Twitter, Weibo, and Facebook), the user has to use some keywords to search for relevant posts. However, gathering a set of representative topical keywords is a very tedious and time-consuming task that often involves a lengthy iterative process of searching and manual reading. In this talk, I first discuss this problem and present an algorithm to help the user discover such search keywords. After searching using the keywords, the resulting set of posts can still be quite noisy because many posts containing the keywords may not be relevant. A supervised learning step is needed to filter out those irrelevant posts. Here I discuss a sampling selection bias problem faced by learning, called negative covariate shift, and present an algorithm to deal with it. Finally, I discuss how lifelong machine learning may be employed to help tackle these problems to make much further improvements.

### Problems

SNS analysis (social science)

** Task **

Sub-task:
- Using Keywords to search
  - representative for the public (for what??)
  - keywords (not allowed)
- tell apart relevant an irrrelevant

### keywords for search

social media is huge for classfication
identify good keywords (???
- good coverage
- time consuming for bad ones

different from keyphrase extraction
- fetch doc manually
- not general words, on specific topic

differ from query recommendation (not user personalized or based on a user profile)

### process

seed keywords -> tweet search -> rank words (from current tweets) -> rerank (the shortlisted candidate)
-> user interaction to continue or stop.

is it a good source for knowledge retrieval (???)

#### rank

entropy
(or LDA to get topic and get words from topic)
(CT & RT(random tweet))

#### rerank
for top ones 

research tweets for each word
count how many tweets contains words in candidate (from last step)
twitter tracking api (200 keywords in one search, tracking is not ranked)

#### double ranking

rank it again

### Negative Covariate Shift

irrelevant posts need classification

**problem**: data not sampled iid in a dynamic social network: $P_{train}(X,Y)\neq P_{test}(X,Y)$

Covariate Shift:
$$
P_{train}(Y|X) = P_{test}(Y|X)  \\
P_{train}(X) \neq P_{test}(X)
$$

negative covariate shift: (negative means data people didn't see before)
$$
P_{train}(Y|X) = P_{test}(Y|X) \\
P_{train}(X^+) = P_{test}(X^+) \\
P_{train}(X^-) \neq P_{test}(X^-)
$$

#### propsed method

data transformation from document space to center-based similarity space (CBS)

center document: f1 f2 f3 ...
trans d to the difference to the center

DS Features: n-gram tf-idf
CBS-features: (Cha, 2007) cos best, gow / low small improvements

**Evaluation** 
Dataset: product reviews (50 products) (avoid to label data manually for tweets)
topic classification

### lifelong machine learning

classical ML (chen and liu 2014)
learning is isolated
but people don't.

LML is suitable for:
keyword discovery: .. (Liu et al., 2016)
classfication: ...

### QA

transfer learning is different to LML (TL no accumulation, LML have **knowledge base**) ( implementation ??






