---
layout: post
title:  "聚类理论笔记"
date:   2016-05-16 18:00 +0800
tags: learning clustering mixed_density_function spectral_clustering
categories: chn learning
math_support: mathjax
---

记录一些基本的基本聚类方法的入门笔记，以便后续查找。阅读材料中的工作仅截止到2007年。后续的一些新进展没有考虑。谱聚类本来也想多写一点，然而感觉对来龙去脉还没有对混合密度这么清楚，等实现了几个算法之后再说吧。

直觉上来说，聚类本身是一种对数据相似的衡量，同一类之间尽量相似，不同类之间尽量不一样。但由于没有标注，哪怕使用相同的数据集，对于“类”的定义根据使用场景也很可能不同。

## 1. 混合密度方法

在使用混合密度的想法中，我们简单假设，一个样本一旦属于某一个类，那么它就是从这个类自己的分布中产生的，每个类可以拥有不同的分布、不同的形式，并且不同类之间的分布相互独立，不会互相影响。而每个样本属于某个类的先验概率也是独立的.  
这部分的详细内容可参见 [1]，这里采用与其相似的记号.

- 样本数据集合 $$ D = \{x_k \mid k = 1, 2, \dots, n\} $$，共 n 个
- 类别集合 $$ \{w_j \mid j = 1, 2, \dots, c\} $$, 共 c 个
- 每个类的先验概率 $$ \alpha = \{ \alpha_j = P(w_j) \mid j = 1, 2, \dots, c \} $$
- 每个类的概率密度 $$ p(x \mid w_j, \theta) $$，其中 $$\theta = \{\theta_j \mid j = 1, 2, \dots, c\}$$

同时由于上述假设存在，不同类的分布相互独立，因此有

$$
p(x \mid w_j, \theta) = p(x \mid w_j, \theta_j)
$$

用简单好记的说法，就是当 $$w_j$$ 出现在条件部分时， $$\theta$$ 和 $$\theta_j$$ 可以互换，否则就只能用 $$\theta$$ 表示

### 混合密度

由于前面的假设可以得出整体的样本的密度函数：

$$
p(x \mid \theta) = \sum_{j=1}^c P(w_j) p(x \mid w_j, \theta_j)
$$

如果我们知道密度函数的形式（比如待会儿我们要假设它是高斯密度），那么对上面这给整体的分布中得出的一个独立同分布数据集，就可以使用参数估计的办法 [2] 来求得这个分布中每个未知参数. 假设使用简单的最大似然方法，容易得到它的对数似然函数：

$$
\begin{align}
l(\theta)
 &= p(D \mid \theta) \\
 &= \prod_{k=1}^n p(x_k \mid \theta) \\

ll(\theta)
 &= \ln l(\theta) \\
 &= \sum_{k=1}^n \ln p(x_k \mid \theta) \\
 &= \sum_{k=1}^n \ln (\sum_{j=1}^c P(w_j) p(x \mid w_j, \theta_j))
\end{align}
$$

为了让似然函数最大，令其对于各变量导数为零（这里就是关于各个参数 $$\theta_j$$ 导数为零），同时由于个分布之间独立，对一个参数求导时其他项直接省略，那么有

$$
\begin{align}
0
 &= \frac{\partial}{\partial \theta_j} ll(\theta) \\
 &= \sum_{k=1}^n \frac{1}{p(x_k \mid \theta)} \frac{\partial}{\partial \theta_j} \sum_{j=1}^c P(w_j) p(x \mid w_j, \theta_j) \\
 &= \sum_{k=1}^n \frac{1}{p(x_k \mid \theta)} \frac{\partial}{\partial \theta_j} P(w_j) p(x \mid w_j, \theta_j) \\
 &= \sum_{k=1}^n \frac{P(w_j)}{p(x_k \mid \theta)} \frac{\partial}{\partial \theta_j} p(x \mid w_j, \theta_j) \\
 &= \sum_{k=1}^n \frac{P(w_j)}{p(x_k \mid \theta)} p(x \mid w_j, \theta_j) \frac{\partial}{\partial \theta_j} \ln p(x \mid w_j, \theta_j) \\
 &= \sum_{k=1}^n \frac{p(x, w_j \mid \theta)}{p(x_k \mid \theta)} \frac{\partial}{\partial \theta_j} \ln p(x \mid w_j, \theta_j) \\
 &= \sum_{k=1}^n p(w_j \mid x_k, \theta) \frac{\partial}{\partial \theta_j} \ln p(x \mid w_j, \theta_j) \\
\end{align}
$$

最后的形式可以简单记为：对每个样本，其后验乘以对数似然的偏导，等于整体的似然函数的偏导.

此外，上面是假设我们知道了先验的概率 $$P(w_j)$$ 的情形，如果连先验也是未知的，即我们一开始也不知道到底类别的概率分布如何，那么就可以扩充一下，把先验的概率也作为未知数之一，因此也需要求关于它的偏导并令之为0。但是，光求偏导是没意义的，因为它们自由度并不是 c，即不同类概率之和为1，所有的先验必须满足这一条件，即求：

$$
\max ll(\theta, \alpha) \\
s.t. \sum_{j=1}^c \alpha_j = 1
$$

构造拉格朗日函数：

$$
LL(\theta, \alpha, \lambda) = ll(\theta, \alpha) + \lambda(\sum_{j=1}^c \alpha_j - 1)
$$

对这个式子求偏导，$$\theta$$ 的部分和上面一样，就不考虑了，直接看 $$\alpha$$. 注意到由于 $$\alpha_j = P(w_j)$$ 其实决定了类分布 $$p(x \mid w_j, \theta_j)$$，后者根本不含 $$\alpha_j$$ 项，因此只是一个系数而已，便有：

$$
\begin{align}
\frac{\partial}{\partial \alpha_j} LL 
 &= \lambda + \sum_{k=1}^n \frac{1}{p(x_k \mid \theta)} \frac{\partial}{\partial \alpha_j} \sum_{j=1}^c \alpha_j p(x_k \mid w_j, \theta_j) \\
 &= \lambda + \sum_{k=1}^n \frac{p(x_k \mid w_j, \theta_j)}{p(x_k \mid \theta)} \\
 &= 0
\end{align}
$$

这里用到一个小技巧，上面有 c 个导数等于0的等式，将两边同时乘以 $$\alpha_j$$ 并分别加起来，就有：

$$
\begin{align}
0 &= \lambda \sum_{j=1}^c \alpha_j + \sum_{k=1}^n \sum_{j=1}^c \frac{p(x_k \mid w_j, \theta_j)\alpha_j}{p(x_k \mid \theta)} \\
0 &= \lambda + \sum_{k=1}^n \sum_{j=1}^c \frac{p(x_k, w_j \mid \theta)}{p(x_k \mid \theta)} \\
0 &= \lambda + \sum_{k=1}^n \frac{p(x_k \mid \theta)}{p(x_k \mid \theta)} \\
0 &= \lambda + n \\
\lambda &= -n
\end{align}
$$

再将 $$ \lambda = -n $$ 代入上面导数为0项中，并两边乘以 $$\alpha_j$$，便有 

$$
\begin{align}
n &= \sum_{k=1}^n \frac{p(x_k \mid w_j, \theta_j)}{p(x_k \mid \theta)} \\
n \alpha_j &= \sum_{k=1}^n \frac{p(x_k \mid w_j, \theta_j)\alpha_j}{p(x_k \mid \theta)}  \\
\alpha_j &= \frac{1}{n} \sum_{k=1}^n P(w_j \mid x_k, \theta)
\end{align}
$$

所以，结果简单来说就是，先验的最大似然估计等于样本后验的平均值.

可以发现，上面动不动两边要乘以 $$\alpha_j$$ 是为了把分子上的似然函数凑出一个联合密度函数来，好和分母或者求和进行处理。

### 高斯混合密度

上面推导的是一般情况，而假设我们知道了混合密度中每一个都是高斯，那么所求参数就变成了高斯的参数. 回忆一下多元高斯的密度函数为：

$$
p(x \mid \mu, \Sigma) = \frac{1}{\sqrt[d]{(2\pi)} \sqrt{\vert\Sigma^{-1}\vert}}\exp(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu))
$$

考虑一下只有 $$\mu$$ 未知的情况，上面偏导等于0的项中，需要求密度函数的对数形式。

$$
\begin{align}
\frac{\partial}{\partial \mu_j} \ln p(x \mid \mu_j, \Sigma_j) 
  &= -\frac{1}{2} \frac{\partial}{\partial \mu_j} (x - \mu_j)^T \Sigma_j^{-1}(x - \mu_j) \\
  &= \Sigma^{-1}(x - \mu_j)
\end{align}
$$

上面涉及到对二次型复合函数求导，具体可以参考 [3] 里的矩阵微分操作记录，这里由于我们需要一个列向量，所以结果反了一下，另外协相关矩阵是对称矩阵，因此最终形式也比较简单。

而如果 $$\Sigma_j$$ 也未知的话我就不知道怎么求了，总之周志华老师 \[4\] 直接给了个结果。  
（20160611补充：搜到一个关于估计协相关矩阵的，将二次型视为一个矩阵的 trace 这招比较犀利：[Wikipedia](https://en.wikipedia.org/wiki/Estimation_of_covariance_matrices )）

用这种方法最后实际是一个 NP 问题，因为需要对所有的聚类情形遍历一遍并找出能让似然最大者。实际都是用 EM 来求的。

注意到上面令梯度等于0后，只有后验、密度函数需要求，而后验可以通过贝叶斯公式换为密度函数和先验，因此通过初始化一个多元参数 $$\mu_j, \Sigma_j, j = 1, 2, \dots, c$$，E 步直接求出后验，M 步再迭代求密度函数的各项未知数.

### k-means

k-means 目标是最小化所有点到各自聚类中心的距离之和，实际上就是最大化 $$\Sigma_j = I$$ 时的似然函数，因此可以视为上面高斯混合密度的一个特例.

同时，据此可以发现 k-means 为什么对并不聚成一坨的数据聚类效果较差，因为 $$\Sigma_j = I$$ 显然是个很强的条件.

此外，对一些非线性可分的数据（比如螺旋线）, k-means 或者混合高斯这样的假设就很不适用了，有人对 k-means 加上了 kernel [5]，使得映射到高维空间后线性可分并且可求内积.

## 2. spectral clustering

### 基本想法

谱聚类 [6] 的思想是把数据点都看成图的节点，图上的边则为两个点的某种相似度，例如可以取高斯函数 $$d(x_i, x_j) = \exp(\Vert x_i - x_j \Vert^2 / (2\sigma^2))$$. 而边可以是全连接、取 k-nn 连接、某个 $$d(x_i, x_j) < \epsilon$$ 邻域内连接等等. 一个类之间的相似度应该比较高，不同类间的比较低.

构建 Laplacian Matrix，标准的 (unnormalized) 的定义为 $$ L = D - W $$，其中 D 是对角阵，每个元素为每个顶点的度，W 为邻接矩阵。

这个矩阵有一些重要特点：

- 行和为0
- 必有一个0特征值和全为1的特征向量
- 是半正定矩阵
- **0特征值的重数是图的连通子图的数目**

前两个容易证，后两个可参考 [6].

如果这个图已经不连通了，就自然成类了，但是现实数据总会连起来的。对聚成两类的情形来说，相当于求一个最小分割，使得被割掉的几条边的权和最小，即能找到一个最小二分割. 

但这种二分割算法往往会找出一个将某远处的点单独为一类，其他所有点为一类，我们希望聚出的类别差那么远，因此可以使用子图的 cardinality(顶点数) 或者 volume(度的和) 来归一化一下.

### 不同做法

此外，对于拉普拉斯矩阵还有不同的解释

- 图的最大分割
- 随机游走
- 摄动分析 [8], Ng 除了提出了摄动分析的解释之外也做了比较好的归一化工作，可以说他对谱聚类的贡献是突破性的

在标准拉普拉斯矩阵基础上，对矩阵做不同归一化 (normalized) 可以有不同的形式：

- $$L_{sym} = D^{-1/2}LD^{1/2}$$，相当于用顶点数归一化
- $$L_{rw} = D^{-1/2}L$$，等价于随机游走的方法，相当于用 volume 归一化

### 与其他关系

由于谱聚类最后实际都是转化为离散优化的问题，最后与求矩阵的 trace 迹有关，有一个推导了 kernel k-means 和 谱聚类的关系的工作，证明了二者的等价 [9].

这意味着当我们求超大矩阵的特征向量特别困难时可以继续用 k-means 的搜索和改良方法.

## 3. 其他方法

- LVQ: 见 [4] ，需要标注数据，通过标注数据来修正，不知道这还算不算聚类
- DBSCAN: 见 [4], 并没有什么特别之处，只是不停地以密度范围聚类而已，只不过工程实现里需要考虑距离索引的问题，可以用 geohash 或者 kd-tree 等

### 更多工作

聚类的其他方法也太多了，例如 2014 年的 Science 上密度峰聚类 [10] 等等，其他方法也有一个 2005 年的 survey 可以查看 [11].

## Bibliography

[1]R. O. Duda, P. E. Hart, and D. G. Stork, Pattern Classification. John Wiley & Sons, 2012.  
[2]从参数估计到线性判别函数. http://libzx.so/chn/learning/2016/03/27/from-estimator-to-linear-discriminant.html 2016.  
[3]Randal J. Barnes, Matrix Differentiation. http://www.atmos.washington.edu/~dennis/MatrixCalculus.pdf   
[4]周志华, 机器学习. 清华大学出版社, 2016.  
[5]B. Schölkopf, A. Smola, and K.-R. Müller, “Nonlinear component analysis as a kernel eigenvalue problem,” Neural computation, vol. 10, no. 5, pp. 1299–1319, 1998.  
[6]U. Von Luxburg, “A tutorial on spectral clustering,” Statistics and computing, vol. 17, no. 4, pp. 395–416, 2007.  
[7]J. Shi and J. Malik, “Normalized cuts and image segmentation,” Pattern Analysis and Machine Intelligence, IEEE Transactions on, vol. 22, no. 8, pp. 888–905, 2000.  
[8]A. Y. Ng, M. I. Jordan, Y. Weiss, and others, “On spectral clustering: Analysis and an algorithm,” Advances in neural information processing systems, vol. 2, pp. 849–856, 2002.   
[9]I. S. Dhillon, Y. Guan, and B. Kulis, “Kernel k-means: spectral clustering and normalized cuts,” in Proceedings of the tenth ACM SIGKDD international conference on Knowledge discovery and data mining, 2004, pp. 551–556.   
[10]A. Rodriguez and A. Laio, “Clustering by fast search and find of density peaks,” Science, vol. 344, no. 6191, pp. 1492–1496, Jun. 2014.  
[11]R. Xu, D. Wunsch, and others, “Survey of clustering algorithms,” Neural Networks, IEEE Transactions on, vol. 16, no. 3, pp. 645–678, 2005.



