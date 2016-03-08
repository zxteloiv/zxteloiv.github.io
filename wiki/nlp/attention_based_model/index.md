---
layout: page
title: attention_based_model
math_support: mathjax
---


### concepts

NTM relies on only the last state

attention is an idea that uses a** weighted combination of input states**

like a memory reference where the access is soft.

[attention and memory in deep learning and nlp](http://www.wildml.com/2016/01/attention-and-memory-in-deep-learning-and-nlp/)

### application

[Show, Attend and Tell](http://arxiv.org/abs/1502.03044) for image labelling

[Grammar as a Foreign Language](http://arxiv.org/abs/1412.7449) for language parsing

[Teaching Machines to Read and Comprehend](http://arxiv.org/abs/1506.03340) for QA

[End-to-End Memory Networks](http://arxiv.org/abs/1503.08895) allow the network to read same input sequence multiple times before making an output, updating the memory contents at each step. 

[Neural Turing Machines](https://github.com/dennybritz/deeplearning-papernotes/blob/master/neural-turing-machines.md) use a similar form of memory mechanism, but with a more sophisticated type of addrdessing that using both content-based (like here) and location-based addressing, allowing the network to learn addressing pattern to execute simple computer programs, like sorting algorithms.

a clearer distinction between memory and attention mechanisms, perhaps along the lines of [Reinforcement Learning Neural Turing Machines](http://arxiv.org/abs/1505.00521), which try to learn access patterns to deal with external interfaces.

### further reading

[2016.1.22 Attention mechanism and application on QA over Knowledge Base](quiver-note-url/A5A83CAB-80C9-445D-815E-816FAA63CE48)

[zhihu question](http://www.zhihu.com/question/36591394)



