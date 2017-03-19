---
layout: page
title: READLIST
math_support: mathjax
---


sampling: http://www.cnblogs.com/xbinworld/p/4266146.html

viterbi: https://en.wikipedia.org/wiki/Viterbi_algorithm
virerbi parse: http://www1.icsi.berkeley.edu/~stolcke/papers/cl95/node30.html

SJTU BASIC Summer School: http://basics.sjtu.edu.cn/summer_school/basics16/download/

Synthetic gradient: http://deliprao.com/archives/187

NLP Segmentation tutorial: http://chuansong.me/n/533306951860

ACL 2016 Tutorial Understanding Short Text:
http://www.wangzhongyuan.com/tutorial/ACL2016/Understanding-Short-Texts/

AMR Specification: https://github.com/amrisi/amr-guidelines/blob/master/amr.md

ICML 2016 Tutorials: http://icml.cc/2016/?page_id=97

Memory Network Tutorial: http://www.thespermwhale.com/jaseweston/icml2016/

Advanced Topics of Mathematics: http://www.cims.nyu.edu/~bandeira/Fall2016.MATH.GA.2830.002.MathDataScience.html

The Role of Word Length in Semantic Topology
https://arxiv.org/abs/1611.04842

On the Convergent Properties of Word Embedding Methods
https://arxiv.org/abs/1605.03956

Multi-lingual Knowledge Graph Embeddings for Cross-lingual Knowledge Alignment
https://arxiv.org/abs/1611.03954

Quasi-Recurrent Neural Networks
https://arxiv.org/abs/1611.01576

What Do Recurrent Neural Network Grammars Learn About Syntax?
https://arxiv.org/abs/1611.05774

Dynamic Coattention Networks For Question Answering
https://arxiv.org/abs/1611.01604

Intelligible Language Modeling with Input Switched Affine Networks
https://arxiv.org/abs/1611.09434

Online Sequence-to-Sequence Reinforcement Learning for Open-Domain Conversational Agents
https://arxiv.org/abs/1612.03929

Path-based vs. Distributional Information in Recognizing Lexical Semantic Relations
https://arxiv.org/abs/1608.05014

lambda calculus
http://worrydream.com/AlligatorEggs/
http://blog.klipse.tech/lambda/2016/07/24/lambda-calculus-1.html
http://palmstroem.blogspot.com/2012/05/lambda-calculus-for-absolute-dummies.html
http://www.inf.fu-berlin.de/lehre/WS03/alpi/lambda.pdf

mxnet:

http://mxnet.io/api/python/symbol.html#executing-symbols
http://mxnet.io/api/python/symbol.html#testing-utility-reference
http://mxnet.io/tutorials/nlp/cnn.html
https://github.com/dmlc/mxnet-notebooks/blob/master/python/tutorials/bucket_io.py
https://github.com/dmlc/mxnet/blob/master/example/rnn/lstm.py
https://github.com/dmlc/mxnet-notebooks/blob/master/python/tutorials/char_lstm.ipynb
https://ljalphabeta.gitbooks.io/mxnet-notes/content/mxnet_python_overview_tutorial.html
http://mxnet.io/tutorials/index.html#id1
http://mxnet-tqchen.readthedocs.io/en/latest/packages/python/tutorial.html
http://mxnet-tqchen.readthedocs.io/en/latest/packages/python/symbol_in_pictures.html
https://indico.io/blog/getting-started-with-mxnet/

wikipedia dump:

https://en.wikipedia.org/wiki/Wikipedia:Database_download#Where_are_the_uploaded_files_.28image.2C_audio.2C_video.2C_etc..2C_files.29.3F
https://meta.wikimedia.org/wiki/Data_dumps
https://en.wikipedia.org/wiki/Wikipedia:Database_download#Static_HTML_tree_dumps_for_mirroring_or_CD_distribution
https://www.mediawiki.org/wiki/Alternative_parsers
https://www.mediawiki.org/wiki/Manual:Importing_XML_dumps
https://www.mediawiki.org/wiki/Manual:MWDumper

**Explaining the Learning Dynamics of Direct Feedback Alignment**
Justin Gilmer, Colin Raffel, Samuel S. Schoenholz, Maithra Raghu, and Jascha Sohl-Dickstein
18 Feb 2017ICLR 2017 workshop submissionreaders: everyoneRevisions
Abstract:     Two recently developed methods, Feedback Alignment (FA) and Direct Feedback Alignment (DFA), have been shown to obtain surprising performance on vision tasks by replacing the traditional backpropagation update with a random feedback update. However, it is still not clear what mechanisms allow learning to happen with these random updates. 
    In this work we argue that DFA can be viewed as a noisy variant of a layer-wise training method we call Linear Aligned Feedback Systems (LAFS). We support this connection theoretically by comparing the update rules for the two methods.  We additionally empirically verify that the random update matrices used in DFA work effectively as readout matrices, and that strong correlations exist between the error vectors used in the DFA and LAFS updates. With this new connection between DFA and LAFS we are able to explain why the "alignment" happens in DFA. 
TL;DR: We interpret DFA as a noisy version of a layer-wise training method we call Linear Aligned Feedback Systems (LAFS). 
https://openreview.net/forum?id=HkXKUTVFl

**GAN**

Stability of Generative Adversarial Networks
http://www.araya.org/archives/1183

Read-through: Wasserstein GAN
http://www.alexirpan.com/2017/02/22/wasserstein-gan.html?utm_content=buffer7f258&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer

JS Divergence: https://en.wikipedia.org/wiki/Jensen–Shannon_divergence  
KL Divergence: https://en.wikipedia.org/wiki/Kullback–Leibler_divergence
Wasserstein Divergence: https://en.wikipedia.org/wiki/Wasserstein_metric

令人拍案叫绝的Wasserstein GAN: https://zhuanlan.zhihu.com/p/25071913

解读GAN及其 2016 年度进展: http://mp.weixin.qq.com/s/zd4NgjoTNmM0N9boPLITJw

**Neural Expectation Maximization**
Klaus Greff, Sjoerd van Steenkiste, Jürgen Schmidhuber
18 Feb 2017 (modified: 21 Feb 2017)ICLR 2017 workshop submissionreaders: everyoneRevisions
Abstract: We introduce a novel framework for clustering that combines generalized EM
with neural networks and can be implemented as an end-to-end differentiable
recurrent neural network. It learns its statistical model directly from the data and
can represent complex non-linear dependencies between inputs. We apply our
framework to a perceptual grouping task and empirically verify that it yields the
intended behavior as a proof of concept.
TL;DR: A framework for clustering that combines generalized EM with neural networks and can be implemented as an end-to-end differentiable recurrent neural network
Conflicts: usi.ch, idsia.ch, supsi.ch, cai.fi
Keywords: Theory, Deep learning, Unsupervised Learning
Authorids: klaus@idsia.ch, sjoerd@idsia.ch, juergen@idsia.ch
https://openreview.net/forum?id=BJMO1grtl

**Preliminary Recommendations on Subcategorisation**
http://www.ilc.cnr.it/EAGLES96/synlex/node1.html

**Reinforcement Learning**
Sutton's book: https://webdocs.cs.ualberta.ca/~sutton/book/ebook/the-book.html

UCL Course: http://www0.cs.ucl.ac.uk/staff/D.Silver/web/Teaching.html

**Decoding**
Decoding as Continuous Optimization in Neural Machine Translation
https://arxiv.org/abs/1701.02854

**VAE**

A Hybrid Convolutional Variational Autoencoder for Text Generation:
https://arxiv.org/abs/1702.02390

**DL**:
https://openreview.net/forum?id=Sy8gdB9xx
Understanding deep learning requires rethinking generalization
Chiyuan Zhang, Samy Bengio, Moritz Hardt, Benjamin Recht, Oriol Vinyals
05 Nov 2016 (modified: 25 Feb 2017)ICLR 2017 conference submissionreaders: everyoneRevisions
Abstract: Despite their massive size, successful deep artificial neural networks can
exhibit a remarkably small difference between training and test performance.
Conventional wisdom attributes small generalization error either to properties
of the model family, or to the regularization techniques used during training.

Through extensive systematic experiments, we show how these traditional
approaches fail to explain why large neural networks generalize well in
practice. Specifically, our experiments establish that state-of-the-art
convolutional networks for image classification trained with stochastic
gradient methods easily fit a random labeling of the training data. This
phenomenon is qualitatively unaffected by explicit regularization, and occurs
even if we replace the true images by completely unstructured random noise. We
corroborate these experimental findings with a theoretical construction
showing that simple depth two neural networks already have perfect finite
sample expressivity as soon as the number of parameters exceeds the
number of data points as it usually does in practice.

We interpret our experimental findings by comparison with traditional models.
TL;DR: Through extensive systematic experiments, we show how the traditional approaches fail to explain why large neural networks generalize well in practice, and why understanding deep learning requires rethinking generalization.

**Reinforcement Learning with Unsupervised Auxiliary Tasks**
Max Jaderberg, Volodymyr Mnih, Wojciech Marian Czarnecki, Tom Schaul, Joel Z Leibo, David Silver, Koray Kavukcuoglu
05 Nov 2016 (modified: 04 Mar 2017)ICLR 2017 conference submissionreaders: everyoneRevisions
Abstract: Deep reinforcement learning agents have achieved state-of-the-art results by directly maximising cumulative reward. However, environments contain a much wider variety of possible training signals. In this paper, we introduce an agent that also maximises many other pseudo-reward functions simultaneously by reinforcement learning. All of these tasks share a common representation that, like unsupervised learning, continues to develop in the absence of extrinsic rewards. We also introduce a novel mechanism for focusing this representation upon extrinsic rewards, so that learning can rapidly adapt to the most relevant aspects of the actual task. Our agent significantly outperforms the previous state-of-the-art on Atari, averaging 880\% expert human performance, and a challenging suite of first-person, three-dimensional \emph{Labyrinth} tasks leading to a mean speedup in learning of 10$\times$ and averaging 87\% expert human performance on Labyrinth.
Conflicts: google.com, ox.ac.uk, ucl.ac.uk
Authorids: jaderberg@google.com, vmnih@google.com, lejlot@google.com, schaul@google.com, jzl@google.com, davidsilver@google.com, korayk@google.com
https://openreview.net/forum?id=SJ6yPD5xg


On the Origin of Deep Learning
https://arxiv.org/abs/1702.07800


