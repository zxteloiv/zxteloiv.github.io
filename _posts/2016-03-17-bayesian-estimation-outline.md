---
layout: post
title:  "贝叶斯估计概要复述"
date:   2016-03-17 21:30 +0800
tags: learning bayesian_estimation bayesian
categories: chn learning
math_support: mathjax
---

从 Duda 的书《Pattern Classification》整理而来的一个轮廓，希望能对理解贝叶斯估计有所帮助。  
新的更宏观的来龙去脉可以看新的一篇:[从参数估计到线性判别函数](http://libzx.so/chn/learning/2016/03/27/from-estimator-to-linear-discriminant.html)

### 1. 贝叶斯决策

#### 靠天

首先我们使用先验来预测，比如海里捕捞的鱼是三文鱼 $$c_1$$ 还是骨鱼$$c_2$$.  
可以简单认为大海里只有均匀分布的骨鱼和三文鱼，并且鱼有个总量，那么概率可以简单以数量得到，分别为 $$P(c_1), P(c_2)$$.

决策：若 $$P(c_1) > P(c_2)$$ 则认为鱼是三文鱼。  
这种判断把所有鱼都认为是一种，那么正确率显然只会有 $$P(c_1)$$ 那么多。

#### 条件概率

这种做法简单粗暴，效果不是很好。如果我们多知道一点信息，考虑鱼的**特征**，例如长度、鱼身颜色，引入特征 $$x \in \mathcal X$$.

决策：若 $$p(c_1 \vert x) > p(c_2 \vert x)$$ 则认为是三文鱼。这里 $$x$$ 是已知的信息。

然而 $$p(c_i \vert x), i = 1, 2$$ 并不是那么好算，使用贝叶斯公式处理。

$$
p(c_i \vert x ) = \frac{p(x \vert c_i) p(c_i)}{p(x)}, i = 1, 2
$$

这样只用比较右边大小就可以了。分母一样可以略去。

**其他**

从略一提。

- 对一个决策方法可以用最小错误率、最小风险等来衡量
- 可以设计一些判别函数来表示决策
- 似然和先验用正态密度有一些性质
    - 等密度点为椭圆，轴由协方差矩阵规定
    - 白化变换化椭圆为圆

### 2. 最大似然估计 MLE

以三文鱼类为例，先验和似然的具体形式实际并不知道的。  
假设知道分布的形式 $$p(x \vert c) = p(x \vert c; \theta) \sim \mathcal N(\mu, \sigma^2)$$，最大化似然为一种估计参数方式。

从该分布中 draw 一个样本集合 $$\mathcal D = \{x_1, \dots, x_n\}$$，再假设知道是独立同分布，则有

$$
\begin{align}
\hat \theta_i 
&= \arg \max_{\theta_i} P(\mathcal D \vert \theta_i) \\
&= \arg \max_{\theta_i} \prod p(x_k \vert \theta_i)
\end{align}
$$

所得 $$\hat \theta_i$$ 就是 MLE 估计值.

可以从众多分布之中找到一个似然最大的可能分布。**数据越多，似然函数的形状越窄**。

顺便一提 MAP 估计则是 $$\hat \theta_i = \arg \max_{\theta_i} l(\theta)p(\theta)$$.

若模型错误，例如被估计的模型为 $$\mathcal N(\mu, 1)$$ 而实际为 $$\mathcal N(\mu, 10)$$，MLE 估计的最优未必会比依然用错误模型的非“最优”参数工作得好.

### 3. 贝叶斯估计 Bayesian Estimation

我们需要知道的是 $$p(x)$$，实际上很难知道这是个什么密度形式，而如果假设知道它是什么固定的分布，但参数不知道，即 $$p(x \vert \mu)$$，这时就能处理.

这时 $$x$$ 是概率密度的变量本身，而不再是模糊的函数。

从该分布中 draw 一个样本集合 $$\mathcal D = \{x_1, \dots, x_n\}$$，再假设知道是独立同分布.

$$
\begin{align}
p(x \vert D) 
&= \int p(x, \mu \vert D) du \\
&= \int p(x \vert \mu) p(\mu \vert D) du
\end{align}
$$

贝叶斯把参数当成有分布的随机变量，而 MLE 把参数当成一个定值。  
一旦知道 $$\mu$$，似然部分的分布是已知的，故只需要求 $$p(\mu \vert D)$$ 部分，同样使用贝叶斯公式好算。这里虽然好像回到了最大似然类似的形式上，但各部分的意义完全不同了，尤其 $$\mu$$ 也是一个随机变量。

$$
\begin{align}
p(\mu \vert D)
&= \frac{P(D \vert \mu) p(\mu)}{P(D)} \\
&= \frac{P(D \vert \mu) p(\mu)}{\int P(D \vert \mu) p(\mu) d\mu} \\
&= \alpha P(D \vert \mu) p(\mu) \\
&= \alpha \prod P(x_k \vert \mu) p(\mu)
\end{align}
$$

分母的归一化因子放到前面不管它。右边都是为未知的分布，都假设它们是一维正态，且方差及 $$\mu_0$$ 都已知。即

$$
p(x_k \vert \mu) \sim \mathcal N(\mu, \sigma^2) = \frac{1}{\sqrt{2\pi}\sigma} \exp(-{(x_k - \mu)^2} / {2\sigma^2}) \\
p(\mu) \sim \mathcal N(\mu_0, \sigma^2_0) = \frac{1}{\sqrt{2\pi}\sigma} \exp(-{(x_k - \mu_0)^2} / {2\sigma_0^2})
$$.

代入前面可以推得

$$
p(\mu \vert D)
= \alpha' \exp ((n + 1)\mu^2 - 2(\sum_k x_k + \mu_0)\mu)
$$

显然这可以凑成正态密度的形式。

最后可以得到

$$
\mu_n = \frac{\sigma_0^2}{\sigma_0^2 + \sigma^2 / n} \bar x_n + \frac{\sigma^2 / n}{\sigma_0^2 + \sigma^2 / n} \mu_0 \\
x_n = \sum_k x_k / n
$$

观察右边的性质：

- 假设 $$n=1$$ 时，就是似然和先验的均值的加权平均。回忆 $$\sigma_0$$ 是先验的分布的方差。
    - 而 $$\sigma_0$$ 越大或者说远超过 $$\sigma$$ 时，表示我们对先验的分布方差就越大，就越不确信先验的均值是多少，因此所得估计就越倾向于似然的均值。
    - 而 $$\sigma_0=0$$ 则说明先验非常准，我们完全信任先验，似然的均值也就无所谓了。
- 而如果 $$n$$ 值增加，我们对似然的均值就会越信任，因此就用 $$\sigma^2$$ 除以之，得到一个倾向于似然的权重。

若是多元分布的情况也类似。

### conclusion

好奇怪... 忘了之前纠结的地方在哪了，又要重新读一遍课本吗 >A<




