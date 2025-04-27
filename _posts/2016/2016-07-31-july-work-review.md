---
layout: post
title:  "七月工作小结"
date:   2016-07-31 21:05 +0800
tags: 
categories: life
math_support: 
---

这个月终于进入了读论文模式，然而大部分点子都还比较琐碎，没有办法串起来。还好暑期还算长，慢慢来。

### 读书

**现代语言学流派 （增订本）** 冯志伟  
老爷子写的又一本神书，几十年前写的到现在还能继续增订再版，国内估计没多少了。

**编程语言实现模式** Terence Parr  
我想说，这本话都讲不利索，逻辑不通，谁知道怎么那么高分的。
上来就堆专有名词，并且不注重概念之间的承接关系，连 LR 都没提一句，大贴代码段，有什么鸟用吗。
气死了，死贵死贵的书还是凑了半天单才勉强活动打折掉10块钱。
然而不幸在读的时候勾画了一句，退不了了，已扔闲鱼。

### 工作

qa system

- 发现 lua 没办法做跨请求的大资源共享，只好把 trie-index 这块代码删了又用 tornado 服务化。翻了翻以前的公司里代码原来也是把 cache 存储交给 redis 去做的，lua + nginx 看来无法实现类似 data unit 的功能了。
- 把整个系统跑了起来，写了个 web 前端

### 学习

parsing：

- shift-reduce tutorial: http://www.cs.binghamton.edu/~zdu/parsdemo/srintro.html
- 一个综合了网上各种资源并结构化为体系的教学站：https://learn.saylor.org/course/view.php?id=74
- LL wikipedia: https://en.wikipedia.org/wiki/LL_parser
- LR wikipedia: https://en.wikipedia.org/wiki/LR_parser
- CKY wikipedia: https://en.wikipedia.org/wiki/CYK_algorithm
- Stanford Compiler open course: http://web.stanford.edu/class/archive/cs/cs143/cs143.1128/
- ACL2013 tutorial - Semantic Parsing with Combinatory Categorial Grammars: https://www.youtube.com/playlist?list=PLun-LUE1uLNvWi-qV-tRHohfHR90Y_cAk

基本能把 parsing 相关的东西捋一捋，文章还是先不贴了，等读差不多了再另写个梳理。

type theory:

- Cardelli, Luca. “Type Systems.” ACM Computing Surveys 28.1 (1996): 263–264. Print.

### 生活

没什么特别的，总体上过得还挺开心。越来越喜欢女孩子有大○了怎么办还有救吗（

