---
layout: post
title:  "从参数估计到线性判别函数"
date:   2016-03-27 12:30 +0800
tags: learning bayesian_estimation bayesian discriminant_function maximum_likelihood_estimation
categories: chn learning
math_support: mathjax
---

对[前一篇](http://libzx.so/chn/learning/2016/03/17/bayesian-estimation-outline.html)的一个补充提纲，实在不知道怎么改前一篇只好另写一个.

对一个决策理论来说，我们主要的想法是看一个样本应该分到某个类.
我们以判别函数来定义它，比如两类 1 和 2 类就有判别函数 $$g_1(x), g_2(x)$$.

一般就看谁的值大，我们就觉得分到那个类里. 当我们不知道 $$x$$ 的任何信息时，
可以直接用常识，比如 1 类和 2 类的全部概率 $$P(c_1), P(c_2)$$.  
即定义 

$$
g_i(x) = P(c_i).
$$

这样错误率会很高，于是我们考虑用 $$x$$ 的已知信息即它的特征.   
重新定义并应用贝叶斯公式

$$
\begin{align}
g_i(x) &= P(c_i \vert x) \\
       &= \frac{p(x \vert c_i) P(c_i)}{p(x)}
\end{align}
$$

对 $$P(c_i)$$ 依然是用到的常识，但是新的一项 $$p(x \vert c_i)$$ 就似乎
要求我们知道 $$x$$ 的真正密度.

但真实的密度我们真不知道. 不过我们预先收集了这个密度中生成的一些样本
即 $$D=\{x_1, \dots, x_k\}$$，并且假设它们是 i.i.d.    
不失一般性，这里可以假设样本都是同一类的，这样可以省去对应的下标和密度的下标.

于是有 MLE 方法：我们假设知道密度的形式，只是某个参数不知道，那么就求参数.   
我们已经有了一堆样本，某个参数可以让我们得到这些样本的"可能性"(似然)最大，
就以该参数值作为我们对真实参数的一个估计. 

$$
\begin{align}
\hat \theta_i 
&= \arg \max_{\theta_i} P(\mathcal D \vert \theta_i) \\
&= \arg \max_{\theta_i} \prod p(x_k \vert \theta_i)
\end{align}
$$

另有贝叶斯方法: 我们知道也假设密度的形式在知道某些参数后就确定了，
也要估计参数. 但是参数是一个随机变量，也是有分布的.   
我们相当于改而求密度的边缘分布，把所有的参数可能性取值积分掉：

$$
\begin{align}
p(x \vert c_i) &= p(x \vert D[, c_i]) \\
       &= \int p(x, \theta \vert D) du \\
       &= \int p(x \vert \theta) p(\theta \vert D) du \\
\text{where } p(\theta \vert D)
&= \frac{P(D \vert \theta) p(\theta)}{P(D)} \\
&= \frac{P(D \vert \theta) p(\theta)}{\int P(D \vert \theta) p(\theta) d\theta} \\
&= \alpha P(D \vert \theta) p(\theta) \\
&= \alpha \prod_k P(x_k \vert \theta) p(\theta)
\end{align}
$$

而我们已经假设出 $$P(x_k \vert \theta)$$ 和 $$p(\theta)$$ 的具体形式，比如正态，
就又能求得估计了.

重新回到开头的式子：

$$
g_i(x) = \frac{p(x \vert c_i) P(c_i)}{p(x)}
$$

前面都是假设右边第一项密度是某种已知形式，通过把样本集合引入进来，再估计出参数值.
也有[无参数的方法](http://libzx.so/wiki/pattern_recognition/Ch4_non_parametric_estimation/)来得到密度函数，
如 KNN 和 parzen window 等，利用已有数据集，在特征空间上直接估计某块区域的密度.

同时如果我们直接建模判别函数 $$g_i(x)$$，而不管他的类密度是什么，那么
[线性判别函数](http://libzx.so/wiki/pattern_recognition/Ch5_linear_discriminant_functions/)就是一种候选办法.


