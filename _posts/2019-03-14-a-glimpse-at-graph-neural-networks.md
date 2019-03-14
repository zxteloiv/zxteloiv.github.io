---
layout: post
title:  "图神经网络一撇"
date:   2019-03-14 22:15 +0800
tags: graph_neural_network
categories: chn
math_support: mathjax
---

本文大概是一篇综述的浓缩，综合了一些感觉比较关键的东西，不涉及图网络的能力分析等很多别的东西，参考了三篇综述[1-3]和其他一些文章，充当一个快速入门。

# 1. 缘起

为什么要有图神经网络？以前我们处理的数据有一些典型特点：图片具有平移等价性；文本句子具有层次组合性；围棋、图片像素等则具有稳定的栅格结构等。CNN、RNN以及多层的神经网络也是建立在这样的一些特性之上。

然而，有很多数据天生就是一个图。例如推荐系统的用户-商品网络、社交网络、知识图谱、交通路线图、有机化合物分子等等。这些图数据与之前的数据有很大区别：它们结构可变，邻居的数目不固定、也不唯一；图之间的边可能是有向边或无向边或异构图；很多图数据规模较大，最好能并行处理。

以往的神经网络基础组件难以处理这样的数据。当然硬要处理也不是不可以，一种方式我们可以遍历图结构得到一个序列，把它当成普通序列用神经网络就能处理[4]。另一种方式则是可以按照图数据的边来传播隐层状态，比如按照句法树自底向上地把隐层状态传到跟节点上[5]。但是呢，图数据中各个邻居并没有明显的有先后顺序（置换不变性），因此这种做法多少有些诟病，亦不直观，所以我们需要设计一种直接针对图数据的模型组件。

需要多提一句的是，图网络并不是计算图。以往的神经网络框架已经把计算过程表示成一个图，而我们这里谈到的图网络是特指针对图数据输入做的网络。因此，像 Tree-LSTM 这类模型（注意不是 Recursive NN[4]）并不是我们的讨论内容。它们构建树的模式是简单地把相邻两个词往上凑一层，再依次直到凑到只有一个为止。而递归网络[4]处理的则是句法树的输入数据，就可以当成图模型了。概括起来就是，如果只是把模型建模成一个隐式的图则不算，只有输入数据本身就是图才算。

# 2. 图网络

图网络并不是一个新事物，结合历史及最新进展可以把它们简单分为三大类。

- 基于谱学习的模型
- 基于空间的模型
- 更复杂的时空模型和非局部模型

## 2.1 基于谱学习的模型

按照惯例，凡是建模时用了图的特征值的方法都可以叫做谱方法。图模型最开始也是这样。

### 2.1.1 原始模型

考虑到卷积层在图像中有显著的应用，那么如何在图数据上（邻居的个数不定）做卷积[6]？

首先我们构造图的拉普拉斯矩阵（尽管它有很多种构造，但论文中就用这个），它一定是正定的，我们接着它做矩阵分解：
$$
L = I - D ^{-1/2}A D^{-1/2} = U\Lambda U^T
$$
其中，A是图的邻接矩阵，D是邻接矩阵每行求和得到的向量构造成对角阵（即对角线是节点的度，其余为0），$$\Lambda$$ 是特征值构成的对角阵，U是特征向量的对应矩阵。

U矩阵各列构成一组正交基，因此可以用于图表示的傅里叶变换。简单起见，假设每个节点有一个标量属性，所有点的标量属性构成一个图的表示 $$x\in R^{\vert V\vert}$$，那么这个向量的傅里叶变换为 $$U^Tx$$，再逆变换为 $$U(U^Tx)$$。

基于此，我们定义图上的卷积操作：
$$
x * g = U(U^Tx \otimes U^T g) = U g_\theta U^T x
$$
其中，g是我们建模的卷积核，theta 是它的参数。

然后我们把一个节点的标量属性扩展为向量，整个图的所有节点就构成了一个矩阵，作为图的表示。同样可以构建它的卷积操作。

$$
x^{k+1}_{:,j} = \sigma(\sum_{i=1}^{f_{k-1}} U \Theta^k_{i,j} U^T X^k_{:,i}) \,\, (i=1,\dots,f_k)
$$

其中 $$f_{k-1}, f_k$$ 分别为输入和输出的卷积通道数目。

由于这种方法要计算拉普拉斯矩阵的分解，并得到特征向量，对于大规模的图，这种方法较难应用。

### 2.1.2 改进

Defferrard 等人[7]则利用了递归定义的[切比雪夫多项式](https://en.wikipedia.org/wiki/Chebyshev_polynomials):

$$
\begin{align}
T_0(x) &= 1 \\
T_1(x) &= x \\
T_k(x) &= 2xT_{k-1}(x) - T_{k-2}(x)
\end{align}
$$

他们提出了 ChebNet 把卷积核的参数定义如下
$$
g_\theta = \sum_{i=1}^K \theta_i T_k(\tilde \Lambda),
$$
其中 $$\tilde\Lambda = 2\Lambda/\lambda_{max} - I_N$$，卷积操作可以简化为计算

$$
x * g = U(\sum_{i=1}^K \theta_i T_k(\tilde \Lambda))U^Tx = \sum_{i=1}^K \theta_i T_i(\tilde L) x
$$

这种形式避免了计算特征向量，加上 $$T_i$$ 是一个 i 阶多项式，L 是拉普拉斯矩阵，乘以 x 得到的每一维其实只用到了 对应点的邻居信息（这点可以仔细考虑一下为什么，类似于用邻接矩阵乘以 x）。卷积操作就只需要对各个点及其邻居进行计算即可完成。

这样的技术改进非常酷炫，相当于每次计算只需要加载邻居数据就行。

Kipf 和 Welling 则进一步提出了 GCN（没错图卷积网络就在这里诞生），它们把上面这个切比雪夫多项式限制到只用一阶、最大特征值近似为2，进一步把计算简化为：
$$
g * x = \theta_0 + \theta_1 (L - I_N) x = \theta_0 x - \theta_1 D ^{-1/2}A D^{-1/2} x
$$
再简化一下，减少参数个数
$$
g * x = \theta(I + D ^{-1/2}A D^{-1/2}) x, \,\, \theta = \theta_0 = -\theta_1
$$

为了避免深度网络的梯度消失，做了个 renormalization

$$
I + D ^{-1/2}A D^{-1/2} = \tilde D ^{-1/2}\tilde A \tilde D^{-1/2}
$$

跟上面一样考虑多个卷积通道，输出变为

$$
Z = \tilde D ^{-1/2}\tilde A \tilde D^{-1/2} X \Theta
$$

这些谱方法虽然可以实现节点计算的局部化，但是依然有很多问题：

- 图的任何扰动都会导致特征矩阵的基发生变化
- 所学的参数不能用于不同的图
- 整个图需要加载到内存中计算
- 拉普拉斯矩阵仅用于无向图

## 2.2 基于空间的模型

回顾图像上的卷积操作，最终实际计算就是用卷积核对一个区域的每个节点都做一个加权和。
图网络的目标就是给出每个节点的表示，我们也直接用它在空间上的邻居做计算就好了。因此这类叫做基于空间的模型。

一般性地将它们定义为

$$
\begin{align}
h_v &= f(x_v, x_{co[v]}, h_{ne[v]}, x_{ne[v]}) \\
o_v &= g(h_v, x_v)
\end{align}
$$

对每个节点的表示，所用的参数只有：点自身的信息、边的信息、邻居的表示、邻居的信息。而输出 o 就直接用节点自己的表示和输入即可。

这种初级的 GNN 使用 Almeida-Pineda 算法[9,10]做优化，它要求 f 是一个[压缩映射](https://en.wikipedia.org/wiki/Contraction_mapping)[11].

当然初级的模式是不行的，类比 RNN 和 CNN 的区别，GNN 又有两种进阶组合：

- 循环图神经网络
- 消息传递网络

### 2.2.1 循环图神经网络

简单来说就是用同一套参数不行地对输入的图表示进行变换，实现循环的过程。就像 RNN 不停用一组参数对输入做变换一样。

常见的实现是基于GRU的方式，成为 Gated Graph Neural Network （GGNN）。

$$
\begin{align}
h_v^0 &= x_v \\
r^t_v &= \sigma(c_v^r \sum_{u\in N(v)} W_r h_u^{(t-1)} + b^r) \\
z^t_v &= \sigma(c_v^z \sum_{u\in N(v)} W_z h_u^{(t-1)} + b^z) \\
\tilde h^t_v &= \rho (c_v \sum_{u\in N(v)} W (r^t_v \odot h_u^{(t-1))} + b) \\
h^t_v &= (1 - z^t_v) \odot h_v^{(t - 1)} + z^t_v \odot \tilde h^t_v
\end{align}
$$

具体应用则可以看用 GGNN 将 AMR 表示转换为自然语言的句子这篇 [12]，这里就不讲了.

### 2.2.2 消息传递网络

消息传递网络(Message Passing NN, MPNN)则类似于 CNN 的组合方式，由不同的图网络层叠而来。主要包括两个步骤（其实也就对应上面说的抽象定义）：

- 消息传递（Message Passing）把邻居的信息传到节点上，类似的操作也有其他文献成为 aggregate 或者 propagation
- 读出（Readout）输出最终的结果。

对于 GCN [8] 来说，它的消息传得就是把周围邻居的信息以固定的权重传到中心节点上来。

$$
h_i^{(l+1)} = \sigma(h_i^{(l)}W_0^{(l)} + \sum_{j \in N(i)} (1/c_{ij})h^{(l)}_j W_1^{(l)})
$$

N(i) 表示 i 的邻居节点集合。而其中的 c_ij 则作为归一化权重，或者是学习的参数。

GCN 的特点可以简单看出来：

- 所有节点权重共享（类似CNN）
- 邻居线性加和，顺序不影响
- 复杂度为边的数目（对应邻居个数）
- 局部性：一个点只需要邻居的信息参与计算

但是，它需要加入Gating机制以便进行深度网络计算，也没考虑边的影响。

而另一种加入边信息考量的网络[13]则跟上面的计算类似，区别无非是先从邻居的信息计算出边、再从边计算出节点自身。它的特点也不难想到：

- 支持边的特征
- 表现力强于GCN
- 支持稀疏矩阵

但是它需要保存边的激活信息作为中间表示，且不好做下采样 (subsampling）因此只能在较小的图上实现。

另一种图注意力网络[14]则把 attention 机制结合进来，动态地用 attention 算节点与邻居的权重，再用这个权来为邻居求和。它不需要存边的激活信息，速度和表现力都介于GCN和考虑边的GCN之间，但是也难以优化。

另外再提一个叫做 GraphSage 的网络[15]，它与 GCN 的区别在于它的邻居是设定最大距离以后随机采样的固定数目。计算量因此得到了保证。同时它聚合不同邻居的信息时，考虑了三种方法：均值；LSTM；池化。其中要提一下 LSTM，它们用随机的邻居顺序来聚合，因为LSTM会受顺序影响，这种方法终归还是有点不自然。

GCN 在 NLP 中的应用可以简单看 [15]，它做一个多文档的文本摘要任务。

GCN 的其他应用也不难想到，因为每个节点都有个表示，那么我们可以做节点的分类、图的分类（对接一个softmax层即可），以及图上的链接预测(Link Prediction)，可以简单地用两个节点表示的点积和阈值来判断两节点之间是否可能存在一条边。

## 2.3 其他模型

高级的模型太细节就不详细说了。

时空模型顾名思义就是除了空间上的邻居也考虑了时间序列上的邻居。

或者用关注机制不止关注邻居，也关注整个图上的任意节点。

## 2.4 GN Block 抽象模型

之前有一篇超长、引用文献上百的综述[17]提到了图网络上的 inductive bias 的问题。这篇文章里提出了一种抽象的 GN Block 作为图网络的基本组件。由于过于抽象，其实也无非是上面提到的几种东西的推广，看看就好：

一个图有节点及其属性、边及其属性、全局属性三种东西。GN Block 内部有三个更新函数和三个聚合函数。

- 首先更新边的隐状态，然后聚合得到边的信息；
- 然后更新点的表示，聚合时再加上边的信息得到点的信息；
- 最后聚合节点的属性以及全局属性。

他们谈这个模型的设计时有几个考虑：

- 表示灵活：虽然实值张量最多，但什么别的表示都行
- 支持关系已知的图结构，如交通、句法树、知识图谱；也可以支持关系需要隐含推断的图结构，如文本语料、程序代码、多智能体系统。
- 内部的结构可以随便配置：三个更新函数和三个聚合函数，自助选用。
- 外部组合方式可任选：层叠、循环、Encoder-Decoder都行。

# 3. 深度图神经网络

常规上大家肯定喜欢多叠一些 GCN 提升效果，但不幸的是图网络可能越叠越差。一个猜测是图上的邻居会以指数级在不同层之间传递噪音。
对于这个麻烦大家有一些尝试：

- 残差网络
- Gating 机制
- Jumping Knowledge Net

下面直接给结论：

残差网络的形式[8]，跟ResNet 一样就是把前一层的输入也加到这层的输出上再做激活，然而这种方式最好的模型是2层，更多了泛化性能会显著降低。

Gating 机制[18]就是再学一个门，对每一层都由这个门控制开启多少比例。然而最好的结果止步于4层。

所谓的 JK 网络[19]则尝试了多种方法来聚合多层的输出：直接拼接、最大池化、LSTM。这些手法各有千秋，也没有明显的效果。

# 4. 总结

图网络还是一个比较年轻的模型，今年的应用可能会井喷。

现在分别有基于 tf 的 graph_net、基于 pytorch 的 pytorch geometric、基于 mxnet 的 DGL 等多个图形库了，有兴趣的同学不妨一试。

更多的内容还请大家关注综述和各篇文献，本文是等待跑程序期间的一个半成品，献丑了请别见怪，O(∩_∩)O。


# 参考文献 

1. Wu, Z., Pan, S., Chen, F., Long, G., Zhang, C., Yu, P.S., 2019. A Comprehensive Survey on Graph Neural Networks. arXiv:1901.00596 [cs, stat].
2. Zhang, Z., Cui, P., Zhu, W., 2018. Deep Learning on Graphs: A Survey. arXiv:1812.04202 [cs, stat].
3. Zhou, J., Cui, G., Zhang, Z., Yang, C., Liu, Z., Sun, M., 2018. Graph Neural Networks: A Review of Methods and Applications. arXiv:1812.08434 [cs, stat].
4. Konstas, I., Iyer, S., Yatskar, M., Choi, Y., Zettlemoyer, L., 2017. Neural AMR: Sequence-to-Sequence Models for Parsing and Generation, in: Proceedings of the 55th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers). Association for Computational Linguistics, pp. 146–157. https://doi.org/10.18653/v1/P17-1014
5. Socher, R., Huval, B., Manning, C.D., Ng, A.Y., 2012. Semantic compositionality through recursive matrix-vector spaces, in: Proceedings of the 2012 Joint Conference on Empirical Methods in Natural Language Processing and Computational Natural Language Learning. Association for Computational Linguistics, pp. 1201–1211.
6. Bruna, J., Zaremba, W., Szlam, A., Lecun, Y., 2014. Spectral networks and locally connected networks on graphs. International Conference on Learning Representations (ICLR2014), CBLS, April 2014 http://arxiv.org/abs/1312.6203.
7. M. Defferrard, X. Bresson, and P. Vandergheynst, “Convolutional neural networks on graphs with fast localized spectral ﬁltering,” in Advances in Neural Information Processing Systems, 2016, pp. 3844–3852. 
8. Kipf and Welling. 2017. Semi-Supervised Classification with Graph Convolutional Networks. ICLR
9. L. B. Almeida, “A learning rule for asynchronous perceptrons with feedback in a combinatorial environment.” in Proceedings of the International Conference on Neural Networks, vol. 2. IEEE, 1987, pp. 609–618.
10. F. J. Pineda, “Generalization of back-propagation to recurrent neural networks,” Physical review letters, vol. 59, no. 19, p. 2229, 1987.
11. M. A. Khamsi and W. A. Kirk, An introduction to metric spaces and ﬁxed point theory. John Wiley & Sons, 2011, vol. 53.
12. Beck, D., Haffari, G., Cohn, T., 2018. Graph-to-Sequence Learning using Gated Graph Neural Networks. Proceedings of the 56th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers) 1, 273–283.
13. Kipf et al. 2018. Neural Relational Inference for Interacting Systems. ICML
14. Veličković et al. 2018. Graph attention networks. ICLR
15. Hamilton, W., Ying, Z., Leskovec, J., 2017. Inductive Representation Learning on Large Graphs, in: Guyon, I., Luxburg, U.V., Bengio, S., Wallach, H., Fergus, R., Vishwanathan, S., Garnett, R. (Eds.), Advances in Neural Information Processing Systems 30. Curran Associates, Inc., pp. 1024–1034.
16. Yasunaga, M., Zhang, R., Meelu, K., Pareek, A., Srinivasan, K., Radev, D., 2017. Graph-based Neural Multi-Document Summarization, in: Proceedings of the 21st Conference on Computational Natural Language Learning (CoNLL 2017). Association for Computational Linguistics, pp. 452–462. https://doi.org/10.18653/v1/K17-1045
17. Battaglia, P.W., Hamrick, J.B., Bapst, V., Sanchez-Gonzalez, A., Zambaldi, V., Malinowski, M., Tacchetti, A., Raposo, D., Santoro, A., Faulkner, R., Gulcehre, C., Song, F., Ballard, A., Gilmer, J., Dahl, G., Vaswani, A., Allen, K., Nash, C., Langston, V., Dyer, C., Heess, N., Wierstra, D., Kohli, P., Botvinick, M., Vinyals, O., Li, Y., Pascanu, R., 2018. Relational inductive biases, deep learning, and graph networks.
18. Rahimi et al. 2018. Semi-supervised User Geolocation via Graph Convolutional Networks. ACL
19. Xu, K., Li, C., Tian, Y., Sonobe, T., Kawarabayashi, K., Jegelka, S., 2018. Representation Learning on Graphs with Jumping Knowledge Networks, in: International Conference on Machine Learning. Presented at the International Conference on Machine Learning, pp. 5453–5462.

