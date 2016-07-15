---
layout: page
title: BinaryNet and Swivel
math_support: mathjax
---


http://mp.weixin.qq.com/s?__biz=MzAwMjM3MTc5OA==&mid=404476249&idx=1&sn=8c1449057d8af89cf0a0cfea8169abdf&scene=0#wechat_redirect

**今天涉及的论文有：**

\1. 《BinaryNet: Training Deep Neural Networks with Weights and Activations Constrained to +1 or -1》

\3. 《Swivel: Improving Embeddings by Noticing What's Missing》

\4. 《Random Walks on Context Spaces: Towards an explanation of the mysteries of semantic word embeddings》

BinaryNet: +1 or -1

这篇论文[1]可以说算是春节期间引起最热烈讨论的论文之一了。论文非常简短精炼，有兴趣的同学可以直接去读原文。这里我来简单概括下论文的贡献。题目已经说明一切，作者们提出在 DNN 中，**我们实际上可以把 weight 和 activation 都进行二值化（binarize）而提高 GPU 运算速度**，使得 training speed 提高7倍以上，并且结果几乎不受影响。具体地，在前向传递过程中，BinaryNet 首先把 weight 和 activation 都二值化后，然后把这些 Binary Matrices 进行了 convolve/dot product，之后再进行了 Batch Normalization（BN）。注意，这里的 BN 后的 parameter 就不再是 binary 的了。因为 BN 的 parameter 并不多，所以不再进行 binarize 也没什么。相反，作者指出，不进行 BN 的话，BinaryNet 的 performance 会有很大下降。关于这点，最近的许多工作中，对于 BN 的重要性也给出了同样的结论。

另外值得一提的是，这篇论文与之前一篇 BinaryConnect[2] 的论文很相似，但是其主要区别有：BinaryConnect 只二值化了 weights，而 BinaryNet 则是 weights + activations 都变成了 binary，这样充分利用了基于 GPU 的 XNOR 和 Popcount 的计算效率，最终使得前向过程被大幅提速。另外，这篇文章还提出了一些其他的 GPU kernel。关于作者们是如何改进 GPU kernel 的，用到的 SIMD 方法等等，可以见原论文 page 6 的左下角，这里就不赘述了。

但是除了这些，这篇论文背后给出的一些 **intuitions** 却更值得思考。第一，个人认为，把 weights 和 activations 都二值化，类似于去模拟人类的思考过程，但是比起决策树这种经典又 powerful 的模型，基于 DNN 的决策网络则富有表现力，更 representative，因为其不再只拥有一个父结点。第二点，二值化后的 weights 和 activations 都是 1-bit precision 的，大大提高了 GPU 计算效率，与此同时节省了计算空间。而实际上，无论是过去的实验还是现在的改进都发现，DNN 中大部分的 node 是处于 inactive 或者 无足轻重的地位。那么，关于这些 node 的计算如何被节省下来？因为毕竟，虽然 BinaryNet 中的 weights 和 activations 是二值化的，但是 gradients 还是多比特的。

Swivel: Improving Embeddings

这篇论文[3]的本质还是在基于共现的 word embedding 上做细节改进，并且大部分都是 Implementation details。不过，既然是 Google 出品，还是引起了不小的关注。

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAIAAACQd1PeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyBpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMC1jMDYwIDYxLjEzNDc3NywgMjAxMC8wMi8xMi0xNzozMjowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNSBXaW5kb3dzIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkJDQzA1MTVGNkE2MjExRTRBRjEzODVCM0Q0NEVFMjFBIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkJDQzA1MTYwNkE2MjExRTRBRjEzODVCM0Q0NEVFMjFBIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6QkNDMDUxNUQ2QTYyMTFFNEFGMTM4NUIzRDQ0RUUyMUEiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6QkNDMDUxNUU2QTYyMTFFNEFGMTM4NUIzRDQ0RUUyMUEiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz6p+a6fAAAAD0lEQVR42mJ89/Y1QIABAAWXAsgVS/hWAAAAAElFTkSuQmCC)

这篇论文中提出的 Swivel，不仅在相关 task 上都超越了 CBOW, GloVe，SGNS 模型，**并且其性能与 corpus 大小无关**，在一定程度上有更好的扩展性。而其最大的贡献，就是改进了一种过去（几乎）从没被考虑过的情况：当两个词（当前词和上下文词）之间，没有共现数据时，是真实语言空间就不存在共现（两个词之间毫无瓜葛）还是语料不足导致的“假无关联”？为了处理这两种不同的情况，Google 将 loss function 改成了 piecewise 的，并且提出了不同的 penalty。只不过，这个 penalty 看起来真是 full of magic，无法理解为什么用这样奇怪的方式做“smoothing”，更无法理解为什么 f(x) 是这么一个很奇特的表达式。只不过，这让我想起了另外一篇论文，《Random Walks on Context Spaces》[4]，将 GloVe，Skip-Gram 等论文中的一些曾经的 trick 给出了一定程度上的数学解释。也就是说，就像 Google 自己说的，尽管他们这个 Swivel 看起来很 adhoc，但是也许有一天可以被科学证明。

另一方面，Swivel 在 implementation 上的改进则主要集中于如何处理巨大共现矩阵，如何分片，如何分布，如何通信。但是，这些改进与具体的 learning procedure 之前的关系并没有被很好地表现出来，恐怕这也不是 Google 所关心的。





