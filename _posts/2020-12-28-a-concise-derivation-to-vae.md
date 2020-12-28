---
layout: post
title:  "一则VAE推导的记忆方法"
date:   2020-12-28 23:00 +0800
tags: variational_inference
categories: chn
math_support: mathjax
---

时隔多年又要把VAE捡起来用，回忆了一下怎么推导，找到一种比较好记的办法。在这里记一下。

### 1. 起点

背景知识就略去了，简单来说VAE是变分推断的一种应用，属于贝叶斯流派，可以很容易训练生成式神经网络模型。

直接应用结果是两个模型，一个编码器 $$q(z\mid x)$$ 和一个生成器 $$p(x\mid z)$$，训练完毕以后两个模型可以各自使用，随机采样一个 z 就能生成一个 x 样本，或者给定一个 x 得到它一组比较好的隐状态 z。
用图模型来叙述的话比较简单，就是 (z) -> ((x)) 后者是可以观测的，在建模时认为 x 其实由一个看不见的 z 生成。

那么训练损失从何开始？说起贝叶斯流派，自然我们就想到后验概率。隐变量 z 也是随机变量，尽管是具体样本中看不见的但我们大概知道它的分布，这是先验概率，而当我们获得了大量样本后再估计它的分布就是后验概率。
所以最合适的起点其实就是从后验概率开始，即 $$p(z \mid X)$$，真实问题中的 z 分布往往无法解析表达，甚至可能是“无法到达的绝对真理”。
为此我们需要有一个近似的可参数化的一簇分布来近似它 $$q_\theta (z \mid X) \approx p(z \mid X)$$，所谓变分(variational)最初的词源也就是这个分布 q 是有参数、可变的。

要衡量两个近似的分布自然就用散度了，VAE中使用KL散度，对于离散的随机变量的具体形式如下

$$
KL\left[q \parallel p\right] = \sum_x q(x) \log \frac{q(x)}{p(x)}
$$

不难看出，这里两个分布的支撑集必须相同，否则0概率就无法定义了。在我们的例子中，就从针对隐变量的两个分布的KL散度开始。

### 2. 过程

推导的主要思想就是把KL散度展开，然后把分布用贝叶斯公式调换一下顺序，并引入隐变量的先验。

$$
\begin{aligned}
KL\left[q(z\mid X) \parallel p(z\mid X) \right]
    &= \sum_z q(z\mid X) \log \frac{q(z\mid X)}{p(z \mid X)} \\
    &= \sum_z q(z\mid X) \left[ \log q(z\mid X) - \log p(z \mid X) \right] \\
    &= \sum_z q(z\mid X) \left[ \log q(z\mid X) - \log p(z, X) + \log p(X) \right] \\
    &= \sum_z q(z\mid X) \left[ \log q(z\mid X) - \log p(z, X) \right] + \sum_z q(z\mid X) \log p(X) \\
    &= \sum_z q(z\mid X) \left[ \log q(z\mid X) - \log p(z, X) \right] + \log p(X)
\end{aligned}
$$

到这里只是一路展开，其中最后一行考虑到z的求和跟X没关系，因此所有z概率相加为1.
接下来我们要将联合概率往上面概率图的方向上靠，即应该先有 z 再生成 X。

$$
\begin{aligned}
    KL\left[q(z\mid X) \parallel p(z\mid X) \right]
        &= \sum_z q(z\mid X) \left[ \log q(z\mid X) - \log p(z, X) \right] + \log p(X) \\
        &= \sum_z q(z\mid X) \left[ \log q(z\mid X) - \log p(z) - \log p(X \mid z) \right] + \log p(X) \\
        &= \sum_z q(z\mid X) \left[ \log q(z\mid X) - \log p(z) \right] - \sum_z q(z\mid X) \log p(X \mid z) + \log p(X) \\
        &= KL \left[ q(z\mid X) \parallel p(z) \right] - \mathbb{E}_{z \sim q(z\mid X)} \log p(X \mid z) + \log p(X)
\end{aligned}
$$

至此就结束了。观察等号右边：
- 第一项就是编码器 q 模型与隐变量 z 的先验分布之间的 KL 散度，可以解释为让编码器的输出与先验尽可能接近，实际操作中先验一般用多元高斯或者 vMF 分布。
- 第二项就是从编码器采样 z 之后用解码器重建 X 得到的对数似然，可以解释为让解码器将隐变量能尽可能把隐变量 z 还原成编码器的输入 X。
- 第三项是唯一可观测的变量 X 的边际似然。

由于散度是非负的，因此上式整个非负，整理一下就得到了原始论文 [1] 中的形式：

$$
\log p(X) \geq - KL \left[ q(z\mid X) \parallel p(z) \right] + \mathbb{E}_{z \sim q(z\mid X)} \log p(X \mid z)
$$

### 3. 优化

由于 X 是唯一能观测到的结果，最简单的优化方式就是最大边际似然 $$\max \log p(X)$$，而由于右边是它的一个下界，即所谓Evidence Lower BOund。
那么只要优化这个ELBO（即 p 和 q 中涉及到的参数，包括先验分布也很可能有参数），就可以实现优化最大似然的效果。

在很多条件生成应用中（例如问答、对话等）最大边际似然并不见得有效，可以参考 [2] 换用其他的优化方法。

当然 VAE 的优化中还有好多别的问题，例如KL项并不能得到较好训练，以及训练时从 q 中采样 z 变量之后很可能不连续等问题，可以阅读原文以参考[重参数化](http://blog.shakirm.com/2015/10/machine-learning-trick-of-the-day-4-reparameterisation-tricks/) [3] 等技巧。除了高斯可以很容易重参数化之外，常用的多项分布也可以用 Gumbel 分布重参数化 [4]，感兴趣的同学可以尝试从密度函数推一推。

虽然 VAE 的技巧已经不再时髦，但也作为一种非常经典的技术深入到了各种应用之中[5,6]，它的推导现在也可以说是一个必备技能。本文的核心就是从后验概率和KL散度出发开始推导，比较好记。

# 参考文献 

1. Diederik P. Kingma and Max Welling. 2013. Auto-Encoding Variational Bayes. arXiv:1312.6114 [cs, stat], December. arXiv: 1312.6114.
2. Dipendra Misra, Ming-Wei Chang, Xiaodong He, and Wen-tau Yih. 2018. Policy Shaping and Generalized Update Equations for Semantic Parsing from Denotations. In Proceedings of the 2018 Conference on Empirical Methods in Natural Language Processing, pages 2442–2452, Brussels, Belgium. Association for Computational Linguistics.
3. http://blog.shakirm.com/2015/10/machine-learning-trick-of-the-day-4-reparameterisation-tricks/
4. Eric Jang, Shixiang Gu, and Ben Poole. 2016. Categorical Reparameterization with Gumbel-Softmax. arXiv:1611.01144 [cs, stat], November. arXiv: 1611.01144.
5. Xuanfu Wu, Yang Feng, and Chenze Shao. 2020. Generating Diverse Translation from Model Distribution with Dropout. In Proceedings of the 2020 Conference on Empirical Methods in Natural Language Processing (EMNLP), pages 1088–1097, Online, November. Association for Computational Linguistics.
6. Hai Ye, Wenjie Li, and Lu Wang. 2019. Jointly Learning Semantic Parser and Natural Language Generator via Dual Information Maximization. In Proceedings of the 57th Annual Meeting of the Association for Computational Linguistics, pages 2090–2101, Florence, Italy, July. Association for Computational Linguistics.



