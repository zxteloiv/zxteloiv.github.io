---
layout: post
title:  "汉语分词方法提纲整理"
date:   2015-11-03 11:00
tags: learning natural_language_processing word_segmentation lexical_analysis
categories: note learning
math_support: 
---

## 基于词表

古典方法(不同的启发规则):

* 正向最大匹配 FMM
* 反向最大匹配 BMM
* 双向最大匹配
* 最短路径方法
    * N-最短路(非统计)
    * N-最短路(统计)：词权由统计模型给出

## 基于统计

### 全切分(生成式)

切出所有可能词，交由统计模型选出最可能的

### 由字构词(区分式)

每个字占词中位置，转换为序列标注问题. HMM/CRF

### 全切分与生成式相结合

Wang et al., 2012
一个实现 [Urheen](http://www.openpr.org.cn/index.php/NLP-Toolkit-For-Natural-Language-Processing/68-Urheen-A-Chinese/English-Lexical-Analysis-Toolkit/View-details.html)

## 其它
### 规则与统计相结合

to be tuned.

### jieba

https://github.com/fxsjy/jieba
