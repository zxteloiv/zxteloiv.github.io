---
layout: page
title: Lecture From Language to Knowledge and Back
math_support: mathjax
---


## Abstract: 

Machines with comprehensive knowledge of the world's entities and their relationships has been a long-standing vision and challenge of AI. In the last decade, huge knowledge bases (aka. knowledge graphs) have been automatically constructed from web data and text sources, and have become a key asset for search, analytics, recommendations and data integration. This digital knowledge can be harnessed to semantically interpret textual phrases in news, social media and web tables, contributing to natural language processing and data analytics. This talk reviews these advances and discusses new opportunities and future challenges.

## Biography:

Gerhard Weikum is a Scientific Director at the Max Planck Institute for Informatics in Saarbruecken, Germany, and an Adjunct Professor at Saarland University. His research spans transactional and distributed systems, self-tuning database systems, integration of data and text, and the automatic construction of knowledge bases. He co-authored a comprehensive textbook on transactional systems, received the VLDB Test-of-Time Award for his work on automatic database tuning, and is one of the creators of the YAGO knowledge base. Weikum is an ACM Fellow and a member of several academies in Europe. He has served on various editorial boards and as PC chair of conferences like SIGMOD, ICDE and CIDR. He received the ACM SIGMOD Contributions Award in 2011, an ERC Synergy Grant in 2013, and the ACM SIGMOD Edgar F. Codd Innovations Award in 2016.

## YAGO

10M entities in 350 K types, 200M facts, 100 lang, 95% acc

## open knowledge IE

### start with pattern-based

pattern extraction - knowledge - new pattern - ...

(noisy, high recall, good enough, low precision, ignorance on homonymy & synonymy)

### constrained reasoning for logical consistency [Suchanek 2009, Nakashole 2011]

use knowledge for joint reasoning

## ??

not everything in the text is in KB itself.

### NER

NER and disambiguation can use KB contextually.

### Entities and Quantities in Tables

unit recognition

## QA

Q to SparQL, dep parse choppted, **ILP with type constraints**

## Q

**how do people design type hierarchy? is a tree sufficient? 
what if entity is missing some hierarchy?**

use WordNet / Wikipedia Category / existing taxonomies from new domain, and align them. Good enough.

**subtle knowledge not written explicit in text?**

refer from more books or visual infomation.

**which KB to use**:

YAGO is strong on types but not facts. Rich content Freebase and wikidata in the future.
And Reference entities in YAGO. Combining with text.

**can serious question really be solved in this way?**















