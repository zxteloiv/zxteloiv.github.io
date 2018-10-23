---
layout: page
title: 2018.10.23 Cross linguality and machine translation without bilingual data
math_support: mathjax
---


题  目 (TITLE)：Cross-linguality and machine translation without bilingual data
讲座人 (SPEAKER)：Prof. Eneko Agirre (University of the Basque Country)
主持人 (CHAIR)：Dr. Jiajun Zhang
时  间 (TIME)：Oct. 23 (Tuesday) 16:00-17:00
地  点 (VENUE)：No.2 Conference Room (3rd floor), Intelligence Building
报告人简介（BIOGRAPHY）：
Machine translation is one of the most successful text processing application. Current state-of-the-art systems leverage large amounts of translated text to learn how to translate, but is it possible to translate between two languages without having any bilingual data? In this presentation we will show that this is indeed the case. We will first map the word embedding spaces of two languages to each other, with and without seed bilingual dictionaries. This allows to produce accurate bilingual dictionaries based on monolingual corpora alone, with the same quality as supervised methods. Based on these mappings, it is then possible to train machine translation systems without accessing any bilingual data.
 
报告人简介（BIOGRAPHY）：
Eneko Agirre is Professor at the University of the Basque Country and member of the IXA Natural Language Processing group. His research focuses on lexical and computational semantics, with applications in information retrieval and machine translation. He has produced more than 100 peer-reviewed articles. He has been president of the ACL SIGLEX, member of the editorial board of Computational Linguistics, and has received a Google research award. He is currently an action editor of the TACL journal.

## Motivation

extending mapping to any pair of language,

some even no supervised data available

## Bilingual embedding mappings

supversied mappings SOTA

Artetxe et al. AAAI 2018
- 5000 seed dict

1. normalization (preprocssing)
2. whitening
3. **orthogonal mapping**
4. reweight
5. De-whitening

evaluation via bilingual dict induction

**why does it work?**

Languages are to a large extent isometric in word embedding space.

### reducing supervision

25 word pairs <- 5000

self-learning, ACL17 mapping -> dictionary -> mapping -> ...

**why does it work?**

implicit objective: $W^* = \arg\max_W \sum_i \max_j (X_{i*} W) \cdot Z_{j*}, s.t.\, WW^T = W^TW=I$

Q: why still seed dict?   
A: avoid poor local optima

### Conclusions

- Simple self-learning method to train bilingual embedding mappins
- matches results of supervised methods with no supervision
- implicit optimization object independent from seed dictionary
- High quality dictionaries:
  - Manual analysis shows that real accuracy > 60%
  - High frequency words up to 80%
- github.com/artetxem/vecmap

https://aclanthology.coli.uni-saarland.de/papers/P18-1073/p18-1073

## unsupervised NMT

end-to-end Google NMT

### from bilingual embeddings to uNMT (ICLR2018)

https://openreview.net/forum?id=Sy2ogebAW

**why does it work**

Early to say, but in intuition:

- mapped embedding space provides info. for k-best possible translations
- enc-dec figures how to combine them

### conclusions

- New research area - uNMT
- Perf up 26 BLEU En-Fr
- Plenty of margin to improve
- Code
  - artetxem/arundreamt
  - to be continue
  
## Final words

- word embedding key for NLP
- Mapping represent languages in common space
- Cross-lingual unsupervised mapping enabled breakthroughs
- Unexplored area in its infancy


