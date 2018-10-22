---
layout: page
title: 2017.10.23 meetup Data to Text Automated Text Generation
math_support: mathjax
---


Q1. deep learning and language models, the future in data to text, (end2end)
  A1. Seq2seq looks the same, how to incorporate variety (use historic knowledge) variation, (may be use knowledge templates as constraints)
Q2. measure to optimize and measure to fluency, (BLEU is not that good)
  A2. 
Q3. data representation (how to deal, lacking type system or not flat table, even semi-structured) slot-based may not handle (in variety)
Q4. document planning is placed directly after content selection
Q5. sentence level scoreing and comparing
  A5. human based, no possible measure available now (maybe in application)
Q6. where to find corresponding sentence data for structured data
  A6. use wikipedia or some available parallel dataset (already great impact), 
  (MS 学术大数据, e.g. related work auto-generation)
  
讲 座 人 (SPEAKER)：林钦佑 研究员(微软亚洲研究院)
主 持 人 (CHAIR)： 赵军 研究员
时    间 (TIME)：）October 23 (Monday), 2017, 14:30
地    点 (VENUE)：No.3 Conference Room (3rd floor), Intelligence Building
报告摘要（ABSTRACT）：
We encounter various kinds of data in daily life, for example, weather conditions, game results, product specifications, physical examination results, stock performance, financial data, etc. How to describe, summarize and communicate these data in effective and appropriate manner become necessary. In this talk, I will introduce an automatic text generation platform Data2Text, four major challenges, a template-based solution, evaluation issues, and demonstrate how Data2Text works.

data 出处、核实 (metadata) (data to text is based on data) *why data to text*
体检报表 -> 解释分析

summarization from essay **from data**
(electronic) games description
intelligent speaker
table understanding need professional knowledge (overseas cosmetics, computer) / not easy to read (intuition)

NN methods:
- data-driven should be exactly the same. (thus maybe use copy.)

template-based: high

Challenge 1: knowledge
_ write like a experts (**last-second shot** like in a basketball or soccer game, but not likely in a tennis game)
- mine template from parallel data (dual learning)

Challenge 2: variety
- with or without the ability to generate a variety of sentences (scores stay close then we can use "squeaked")
- same data and different but all valuable sentences

Challenge 3: insight
- **season-high scores** are based on historic data (by which we can say the historic)
- *sometimes we even can talk about the metadata*, **massive RAM** is based on time

Challenge 4: context
- by different stands we say the different thing
- persuasion (扬长避短)

doc planning
document planning (how to say)?

**把写作的知识放在阳光下**

production data set (amazon, Computer & Tablets)

use BLUE / F1 to select what to say and decide how to say

evaluation: fluency, ?, accuracy even significant

Application:

- trained domains:
  - sports news
  - car description
  - production description
  - weather report
- ongoing
  - generate text for data analysis (Power BI)
  
  
![IMG_5175.JPG](resources/B228B0BC15763A88DF1891890E85FFED.jpg =1680x1060)


