---
layout: page
title: Survey Question Answering Papers
math_support: mathjax
---


## readings

### 吴友政 and others - 2006 - 汉语问答系统关键技术研究

**流派和问题**

- 基于检索
  - 容易扩展到较大数据集
  - 无法回答推理任务
- 基于模式匹配
  - 对特定模式问题较好
  - 模式不足、不能表达长距离和复杂关系
- 基于深层 NLP 
  - 可进行一定推理
  - NLP 技术不成熟，除浅层技术之外难以实用
- 基于统计翻译
  - 依赖训练语料，难以获取
  
**framework**

提问 -> 提问处理 <- 答案分类体系

提问处理 -> 查询 -> 检索 <- 语料库

检索 -> 相关反馈 -> 检索

提问处理 -> 答案类型 -> 答案抽取 <- 检索

抽取 -> 答案

**NLP technique**

- 提问分类
- 语义标注 (NER 等)
- 句子检索 (基于主题模型检索)
- 答案匹配 (问答)
- 短语结构、依存分析
- 逻辑形式转换
- 词汇链 (词义归一)
- 复述 (句子归一)

### Yu et al. - 2015 - Empirical Study on Deep Learning Models for QA

**approaches**

- NLP pipeline
- build and infer on large knowledge bases
- machine "reading"

**comparision**

- MemNN
- Neural Machine Translation
- Neural Turning Machine

### Bordes et al. - 2014 - Open question answering with weakly supervised embedding models

**current approaches**

- logical forms (large amount human-labeled data)
- database queries (lexicons and grammars)

**new way**

representation learning

### Zhang et al. - 2016 - Learning Distributed Representations of Data in Community Question Answering for Question Retrieval

focus: community QA, for similar questions searching

- traditional models BM25, LMIR: similar question meanings share few common words
- Language Models with Category smoothing(LMC): matches semantically similar questions, but not clear how semantic relations of words are modeled.
- Translation based models go beyond the word-level matching but cubic time estimation algorithm w.r.t vocabulary size

### Dong et al. - 2015 - Question answering over freebase with multi-column CNN

multiple aspects to understand a question (multi-column), (QA pair & question paraphrases)

**approaches**

- semantic parsing: question into logical forms
  - QA-pair as weak training signals
  - predefined set of lexical triggers are limited
- information extraction over KB (Yao, Bordes)
  - retrieve candidates from KB
  - depend on rules or dep parsing for hand-crafted features (Yao)
  - summation of question word embeddings to represent questions, no order info (Bordes)
  
**novelty**

- multi-column for multi-aspects of a question
- score layer to rank candidates
- no manually annotated logical forms or features
- no pre-defined lexical triggers and rules

### Xu et al. - 2016 - Enhancing Freebase Question Answering Using Textual Evidence

**novelty**

use relation extraction from wikipedia text as evidence, to help KB-retrieval deal with constraints

**approaches**

- semantic parsing
  - lots of annotation containing compositional structures
  - mismatches between grammar-prediected structures and KB structure
- information retrieval from KB
  - using relation extraction (Yao) or using distributed representations (Bordes)
  - hard to handle compositional questions and constraints

### Zhang and Lee - 2002 - Web Based Pattern Mining and Matching Approach to QA

**approaches**

- sophisticated linguistic knowledge and tools (parser, NER, ontology, wordnet, ...)
- web patterns

**novelty**

learn textual patterns from web

**framework**

QA example -> transform -> recognize <- question templates

recognize -> learning <- search engine

learning -> textual patterns -> answering <- recognize (with question)

### Jurczyk and Choi - 2016 - Multi-Field Structural Decomposition for Question Answering

**approaches**

- NLP side: analyze linguistic aspects
  - catch deep semantic meaning
  - graph matching or predicate logic is not scalable to big data
- IR side: searching like patterns
  - scalable for big data
  - not concern with the actual meaning of the context
  
**framework**

documents -> nlp tools -> field extractor -> index
questions, docs -> nlp -> field extractor -> answer ranker <- query indexes

### Bordes et al. - 2014 - Question answering with subgraph embeddings

**method**

- learns low-dimensional embeddings for words and KB-constituents
- embeddings used for scoring questions and answers
- training uses Q and A(in structual representation) pairs

**approaches**

- IR-based
- semantic parsing

both require experts to hand-craft lexicons, grammars, and KB schema to be effective

No annotation:

- A. Fader, L. Zettlemoyer, and O. Etzioni. Paraphrase-driven learning for open question answering. In Proceedings of the 51st Annual Meeting of the Association for Computational Linguistics, pages 1608–1618, Sofia, Bulgaria, 2013.
- A. Bordes, J. Weston, and N. Usunier. Open question answering with weakly supervised embedding models. In Proceedings of ECML-PKDD’14. Springer, 2014.

the latter outperformed the former in some simplified setting with even less supervision.

the latter trains a low-dimensional vector

**novelty**

- more sophisticated inference procedure
- richer representation of answers (with QA path and surrounding subgraph of KB)

**assumption**

- with traning set and KB-provided answer structure
- all answers are entities in KB
- questions include words of one identified KB entity

### Yang et al. - 2014 - Joint Relational Embeddings for Knowledge-based Question Answering

transforming question to logical form.

**novelty**

using semantic relation between lexical repr. and KB-properties

tranditional semantic parsing methods

- language meaning may differ
- NER type may be not consistent with logical forms

**approaches**

- weakly supervised semantic parsing
- connect free texts and KB, using relational learning method (Bordes. 2012, Weston. 2010, 2013)

### Ng and Kan - 2015 - QANUS An Open-source Question-Answering Platform

**approaches**

L. Hirschman and R. Gaizauskas. 2001. Natural Language Question Answering: The View From Here. Natural Lan- guage Engineering, 7, Issue 4:275–300, December.

- IR-based, lexical and syntactic form, no semantics

Jimmy Lin. 2007. An Exploration of the Principles Underlying Redundancy-Based Factoid Question Answering. ACM Transactions on Infor- mation Systems, 27(2):1–55.

ARANEA

- not general QA platform
- factoid only

QANDA

- not publicly available

**frameworks**

Old:

1. documents preprocessing
2. question analysis
3. candidate selection -> document analysis -> answer extraction
4. generation



## framework

## semantic parsing

## representation

## IR-based

## pattern







