---
layout: page
title: 2017.01.11 SE meetup
math_support: mathjax
---


## 搜索引擎的技术趋势和精准度提高

题    目 (TITLE)：搜索引擎的技术趋势和精准度提高
讲 座 人 (SPEAKER)：常毅 副总裁 (华为美国研究所)
主 持 人 (CHAIR)：赵军 研究员
时    间 (TIME)：2017年1月11日，上午10:00
地    点 (VENUE)：智能化大厦三层，第三会议室

## 报告摘要（ABSTRACT）

互联网搜索行业具有后发劣势，而搜索精准度是互联网搜索引擎的核心竞争力。 基于机器学习的排序算法是提高搜索精准度的高效手段，然而，提高搜索引擎的精准度不仅仅只是排序学习， 而是一个系统工程。
该报告将介绍该领域的历史，现状及其最新发展，并详细介绍提高搜索精准度的实践经验和最新研究成果：从排序学习算法、多种语义匹配特征、查询重构这三个角度出发，提高搜索引擎的精准度（该研究成果获得KDD 2016最佳论文奖）。

## 报告人简介（BIOGRAPHY）

常毅博士，现任现任华为美国研究所技术副总裁，华为公司泊松实验室负责人。他于2006-2016就职于雅虎研究院，任研究总监，负责其搜索研究部门,主持雅虎公司的几乎所有重要搜索产品的应用研究和精准度提高。
他在国际一流会议和期刊上发表100余篇论文，并发表了一本专著，30余项专利，并荣获了WSDM 2016最佳论文奖, KDD 2016最佳论文奖。他现担任TKDE副主编,WSDM 2018大会主席。
更多介绍见个人主页 www.yichang-cs.com

## Background

...

## Ranking

...

## Practices

- to avoid UGLY results on top results
- semantic gap between query and document (schema?)
- ???

### ranker

core ranking (point-wise) (avoid bad results at top positions)

1. GBDT + logistic loss (binary classification)
2. Logistic Rank: tell apart perfect and good

different: LogisticRank > LambdaMART (2015): +5% DCG5, over 99% query-URL pairs are labeled bad, point-wise classification is better

### QRW

MT: query and clicked title pair (high freq)

blend query and rewritten query as input

### Feature

click similarity (query and doc in semantic space)

translation

Deep:

deep neural network [CIKM2013, Li Deng]





