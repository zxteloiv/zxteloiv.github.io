---
layout: post
title:  "概率方法小结"
date:   2019-01-02 20:05 +0800
tags: probabilistic_methods random_algorithm algorithm
categories: chn
math_support: mathjax
---

本学期一直在上计算所的《概率方法与随机图》课程，原本以为它是“概率图模型”，结果发现其实是一门讲概率和算法的课。但也多亏这一巧合认识了这是多门有趣的一个内容，和高级算法有一些联系，本文是课程一部分的总结，在本文最后会给出一些参考资料。由于英文世界已经有足够多的相关资料了，本文用中文写。本文的预备知识只需要基本的概率论和一点算法知识，具体来说是随机变量、期望和方差的定义和性质，以及了解最大割问题和图论的基本概念。

**目录**
1. 什么是概率方法
2. [计数](#sec2)
3. 一阶矩方法
    1. [一阶矩方法的形式](#sec3-1)
    2. [利用一阶矩方法设计去随机化算法](#sec3-2)
    3. [对随机的解做小的修正](#sec3-3)
4. [二阶矩方法](#sec4)
5. Lovász 局部引理
    1. [对称的局部引理](#sec5-1)
    2. [不对称的局部引理](#sec5-2)
    3. [构造算法](#sec5-3)
6. [小结](#sec6)

### 1. 什么是概率方法

概率方法在这里其实是最核心的内容，它的主要想法甚至可以单独拿出来只用一篇文章讲，这个方法的精妙之处在于我们可以用“随机”这种听上去就乱来的方式做出严肃的证明并设计算法，甚至很多任务的最好的算法都是基于概率方法设计的。

**存在性证明**

概率方法的核心基于这样一个构想：如果已知一个箱子里有红色球的概率大于0，那么这个箱子里一定存在红球。

这几乎是一句废话，但是这向我们展示出了证明存在性问题的一个办法（网友Xana姥爷有云：宛若违反校规一般的快感）。即若想证明任何一个属性的对象存在，我们首先构造出一个概率空间，然后证明满足这个属性的概率大于0，也就完成了证明。

一旦接受了这个设定，我们就能想到更多的套路。除了一个随机变量的概率，我们还可以利用期望、方差等等统计量，后面几节就依次总结。

**算法设计**

但是仅仅止步于此是不够令人满意的，我们的最终目标还是设计出算法：既然证明了一个东西存在，如果能找出来那就更好了。

为了得到满足该属性的对象，我们通过不停地随机采样，再去验证这个东西符不符合要求，直到随机得到的东西满足条件为止。由于这个对象的概率已经证明大于0，可以确切地计算出来。实际上我们需要尝试的次数就是一个几何分布，期望次数就是此概率的倒数。这种尝试、验证、再尝试的方法又称拉斯维加斯算法（Las Vegas algorithm）。

当然，如果满足目标性质的对象的概率实在太低，尝试的次数就会太多。我们可以对算法进行一些改良，先随机得到一组解，再稍微改一改，让它更可能符合要求。

然而，由于这种先随机一下的方法太过放纵，也许不能让有强迫症的朋友满意，在一些情况下我们可以进一步设计出传统的确定性的算法，只不过其设计方法则是基于概率方法的。

（本文不涉及蒙特卡洛方法，更多可以看参考资料）

<a id="sec2"></a>

### 2. 计数

第一种方法比较简单，也就是我们确切地去计算一个事情的古典概率，如果能算出来大于0，或者它的补集事件概率小于1，那就完成了证明。

考虑这样一个问题，我们知道一个大型体育比赛往往会进行小组赛，出线以后再进行角逐。小组内的队伍一般不可以太多，除了一届比赛时间有限之外，还会有另外一个现象：出线的所有队伍可能会都输给一个没出线的队，从而造成争议。实际上，当队伍增多，争议几乎是必然产生的。我们可以证明，假设一场比赛的胜负可能性是1:1，定义 $$n$$ 支队伍中要选出 $$k$$ 支队伍出线，记作出线集合 $$S$$，事件 $$A_S$$ 表示没有任何队伍同时战胜了这 $$k$$ 个出线队伍，其概率为

$$
\Pr(A_S) = (1 - (\frac{1}{2})^k)^{n-k}
$$

而不论我们基于什么方法，任意地选择出线的队伍，都无法避免争议。事实上，无争议的概率为

$$
\Pr(\cup A_S) \le \sum \Pr(A_S) = \binom{n}{k} (1 - 2^{-k})^{n-k} = o(\frac{1}{n})
$$

即都比 1/n 的阶要低。上面第一个小于号使用了叫做 union bound 的不等式，当只有两个事件做并集时可证明如下：

$$
\Pr(A\cup B) = \Pr(A) + \Pr(B) - \Pr(A\cap B) \le \Pr(A) + \Pr(B)
$$

直观上可以这么想象：一堆事件做并集的概率，总要等于每个事件概率之和，再挖掉它们共同覆盖的部分，而如果不挖掉，就加多了，因此小于等于号成立。这个不等式非常简单，但是极其有用，甚至有时能得到比更高级的比如切比雪夫不等式或者 Hoeffding Bound 等更紧的界。

回到小组赛出线的例子，由于无争议的概率在 k 固定的情况下急剧减小，因此有争议的概率渐进地收敛于1，固然也就大于0，也就一定存在。

如果要设计算法，由于这个有争议的概率足够大，想要找到一种有争议的情况，直接随机地生成一组比赛的胜负情况，几乎必定能找到这样的争议情形。因此这里的算法就是直接随机采样即可。

### 3. 一阶矩方法

<a id="sec3-1"></a>

#### 3.1 一阶矩方法的形式

一阶矩里最常用的就是期望，使用期望的概率方法会利用一个更加巧妙的思路：一个随机变量 X 的期望为 c，那么一定存在 X > c，也一定存在 X < c。直观上理解，期望是一个“平均”的量，如果没有比期望还大的取值，那平均以后就不可能等于现在的期望，反之亦然。

还有一个方法更进一步利用了马尔可夫不等式（Markov Inequality），对于非负随机变量有，$$\Pr(X\ge a) \le EX / a$$。可以简单证明如下，

$$
\begin{align}
EX
&= \sum_{x<a} x\Pr(X=x) + \sum_{x\ge a} x\Pr(X=x) \\
&\ge \sum_{x\ge a} x\Pr(X=x) \\
&\ge \sum_{x\ge a} a\Pr(X=x) \\
&= a\Pr(X\ge a)
\end{align}
$$

由于这个不等式中只出现了期望，我们可以利用它得到一些更方便一阶矩方法中使用的形式，当随机变量是离散的时候，

$$
\Pr(X \ne 0) = \Pr(X > 0) = \Pr(X \ge 1) \le EX / 1 = EX
$$

接下来考虑这样一个简化的最大割的例子，给定一个图 $$G(V, E)$$，有 m 条边，想找到一个分法将所有顶点分到两个集合 A 和 B，使得连接 A 和 B 的所有边的权重之和最大（假设所有的边权都是1，就变为求边数目最多）。但是，最大割是一个 NP-Hard 的问题。不过我们可以证明，最大的割一定包含超过一半的边。

由于不知道如何划分两个点集，我们就乱分：把每个点随机且独立地以 1/2 概率分入某一个集合中）。对每条边定义一个指标随机变量 $$X_i, i=1,\dots m$$，当这条边的两个顶点各在不同的集合中时取1，否则取0。可以求得指标变量之和（即连接两个点集的总边数）的期望。

$$
\mathbb{E}[X]
= \mathbb{E}[\sum_i X_i]
= \sum_i\mathbb{E}[X_i]
= m\Pr(X_i=1) = m(1/2 * 1/2 + 1/2 * 1/2) = m/2
$$

由于期望等于 m/2，因此必定存在一个割集大于 m/2，证毕。不难发现，上面第二个等号利用了期望的线性性质，在一阶矩的概率方法中，这对我们非常有帮助。

<a id="sec3-2"></a>

#### 3.2 利用一阶矩方法设计去随机化算法

对于算法设计，这里继续用上面的例子描述一种设计确定性算法的方法。简单来说，我们刚才求得了一个割集的期望是 m/2，简单记作 $$E[C(A, B)]= m/2$$。这是通过随机地分配所有顶点的所属集合获得的。现在我们改变思路，将顶点一个一个地分配集合，记成 $$E[C(A, B)\mid x_1, \dots, x_n]$$。

用归纳假设。首先考虑第一个顶点，其期望用全概率展开为

$$
m/2 = E[C(A, B)] = \frac{1}{2} E[C(A, B) \mid x_1=A] + \frac{1}{2} E[C(A, B) \mid x_1=B]
$$

由于第一个点等概率地分到 A 或 B，一定有一个的期望会偏大，另一个会偏小，因此，

$$
max(E[C(A, B) \mid x_1=A], E[C(A, B) \mid x_1=B]) \ge E[C(A, B)] = m/2
$$

不妨记较大者取值为 $$v_1$$。

假设我们定义假设已经确定了前 k 个顶点，接下来确定第 k+1 个，同理，

$$
max(E[C(A, B) \mid x_1=v_1, \dots, x_k=v_k, x_{k+1}=A],
    E[C(A, B) \mid x_1=v_1, \dots, x_k=v_k, x_{k+1}=B]) \\
    \ge E[C(A, B) \mid x_1=v_1, \dots, x_k=v_k] \ge m/2
$$

这样就归纳完毕，我们通过每一步依次找最大期望的取值，获得了一个较大的顶点划分，即

$$
m/2 \le E[C(A, B) \mid x_1=v_1, \dots, x_n=v_n]
$$

回过头来看，每一步最大期望意味着什么？在这个例子中，期望就是这个割集的大小，即连接 A、B 两个集合的边的数目。当我们已经赋值了前 k 个顶点，假设第 k+1 个点将分入 A 集合，则我们可以确定性地计算出这些点对于割集的贡献：即有多少相邻顶点分属两个集合。

并且，考虑所有其他尚未赋值的顶点，对于与它们有关的边，无论另一个点是否已被赋值，这条边对割集大小的期望的贡献都是 1/2。这一点可以从上面计算总期望的式子进行验证。因此，无论第 k+1 个点分入哪个集合，未赋值点对期望的贡献都是一样的，在找每一步最大期望时也就可以忽略。

至此，我们发现，每一步求最大期望只需要考虑已经赋值的点，以及即将赋值的点，也就是说，如果第 k+1 个点与当前已赋值的点在某一侧（例如A集合这一侧）拥有的邻居较少，就直接将其分入那一侧。

重新叙述我们的确定性算法：从零开始构建两个点集 A 和 B，贪心地将每个点分到邻居比较少的那个点集中，分完为止。这样得到了一个普通的贪心算法，是不是想拍案称奇？

**一些小结**

但需要补充两点：第一，这个算法选取点的顺序对最终效果肯定是有较大影响的，这也是可以改进的地方之一；第二，对于每一步我们没有选中的另一种取值，将来会如何发展我们是完全未知的，也许其中藏着一个更好的解，但是我们没有任何线索可以找到它。我们只能知道选择最大期望的这条路径走到最后，能找到一个接近最优并有可能是最优的解，它至少在我们所求得的下界之上，但我们并不知道它是否最好。

总结一下，能做去随机化的算法设计有两点要求
- 能用一阶矩方法证明存在性
- 每一步的期望都必须收敛、可以计算

> 稍微发散一下思路，我们可以将这部分算法设计方法和动态规划、强化学习相结合，设计出一套不错的启发式搜索算法。有兴趣的朋友欢迎来讨论。

<a id="sec3-3"></a>

#### 3.3 对随机的解做小的修正

前面说过如果一个随机的东西符合要求的可能性太低，也许我们可以稍微改一改让它变成一个更好的解。这个方法不一定总能用，需要灵活地看。

考虑一个 x-完全图 $$K_x$$，即有 x 个顶点并且每两个顶点之间都有一条边。现在我们对每条边随机并独立地染上红蓝两种颜色，想证明存在一种染色方案，它能保证这个图中没有任何一个所有边颜色相同的 k-完全子图，其中 $$x=n-\binom{n}{k}2^{1-\binom{k}{2}}$$，n 为任意的整数。

不难发现这个要求中 x 是 n 减去了一个量，因此我们可以假设先得到一个 n-完全图，再删掉一定数目的顶点。这也是这里“先随机、再改改”的方法基础。

定义事件 $$A_i$$ 为指标变量，当 n-完全图中第 i 个 k-完全子图只有一种颜色的边，则取1，否则为0。整个 n-完全图中这样的单色 k-完全子图的期望数目则为

$$
\mathbb{E}[\sum_i A_i] = \sum_i \mathbb{E}[A_i] = \binom{n}{k} 2^{1-\binom{k}{2}}
$$

对于每一个单色 k-完全子图，我们都删掉一个点，以及其对应所有边。这样得到的图就是所要求的 x-完全图，并且不包含任意单色的 k-完全子图。这个删除后的图的期望节点数目为：

$$
n - \mathbb{E}[\sum_i A_i] = n - \binom{n}{k} 2^{1-\binom{k}{2}} = x
$$

<a id="sec4"></a>

### 4. 二阶矩方法

常用的二阶矩也就只有方差，而有方差的不等式固然就会想到切比雪夫不等式（Chebyshev Inequality），即 $$\Pr(\lvert X - \mathbb{E}[X]\rvert\ge a) \le Var[X]/a^2$$，利用马尔可夫不等式可直接得到，

$$
\Pr(\lvert X - \mathbb{E}[X]\rvert\ge a) = \Pr((X - \mathbb{E}[X])^2 \ge a^2) = \mathbb E (X - \mathbb{E}[X])^2 / a^2 = Var[X]/a^2
$$

为了方便我们也利用它证明一个特殊情形：

$$
\Pr(X = 0) \le \Pr(\lvert X - \mathbb{E}[X]\rvert\ge \mathbb{E}[X]) \le Var[X] / (\mathbb{E}[X])^2
$$

第一个不等式不难得出，因为 X = 0 一定包含在右边的事件中。

接下来考虑一个随机生成的图 G，共有 n 个顶点，每两个顶点之间以 p 的概率生成一条边。证明当 $$p < \ln n / n$$ 时，图中有孤立点的概率渐进收敛到1。孤立点也就是没有边连接的点。

定义 $$A_i$$ 为指标变量，当第 i 个点为孤立点时取 1。那么这个图中没有孤立点的概率为，

$$
\begin{align}
\Pr(\sum_i A_i = 0)
   &\le \frac{Var[\sum_i A_i]} {(\mathbb{E}[\sum_i A_i])^2} \\
   &= \frac{ \sum_i Var[A_i] + \sum_{1\le i, j \le m, i\ne j} Cov(A_i, A_j) } 
       {(\sum_i \mathbb{E}[A_i])^2} \\
   &= \frac{ \sum_i (E[A_i^2] - (E[A_i])^2) + \sum_{1\le i, j \le m, i\ne j} Cov(A_i, A_j) } 
       {(\sum_i \mathbb{E}[A_i])^2} \\
   &= \frac{ \sum_i (E[A_i] - (E[A_i])^2) + \sum_{1\le i, j \le m, i\ne j} Cov(A_i, A_j) } 
       {(\sum_i \mathbb{E}[A_i])^2} \\
   &\le \frac{ \sum_i E[A_i] + \sum_{1\le i, j \le m, i\ne j} Cov(A_i, A_j) } 
       {(\sum_i \mathbb{E}[A_i])^2}
\end{align}
$$

然后就有点麻烦，要计算各个期望、方差、协方差等统计量，

$$
\mathbb E[A_i] = \mathbb E[A_i^2] = (1-p)^{n-1} \\
Var[A_i] = \mathbb E[A_i^2] - (\mathbb E[A_i])^2 = (1-p)^{n-1} - (1-p)^{2n-2} \\
Cov(A_i, A_j) = \mathbb E[A_iA_j] - \mathbb E[A_i]\mathbb E[A_j] = (1-p)^{2n-3} - (1-p)^{2n-2}
$$

代入上面的不等式，可得

$$
\begin{align}
\Pr(\sum_i A_i = 0)
  &\le \frac{ \sum_i E[A_i] + \sum_{1\le i, j \le m, i\ne j} Cov(A_i, A_j) } 
       {(\sum_i \mathbb{E}[A_i])^2} \\
  &= \frac{n (1-p)^{n-1} + \binom{n}{2} (1-p)^{2n-3}p}
  		{(n (1-p)^{n-1})^2} \\
  &\le \frac{1}{n(1-p)^{n-1}} + \frac{p}{1-p}
\end{align}
$$

随着 $$p < \ln n/n, n\to\infty, \Pr(\sum_i A_i = 0)\to o(1)$$。因此，存在孤立点的概率就渐进趋近于1，命题得证。

> 除了切比雪夫不等式之外，我们还可以用矩母函数以及对应得到的 Chernoff Bound / Hoeffding Bound 等等不等式，获得所求概率更加精细的界。但是本文从略。

### 5. Lovász 局部引理

Lovász Local Lemma(LLL) 是 [László Lovász](https://en.wikipedia.org/wiki/László_Lovász) 在证明一个别的定理时顺带证明的引理，但随着研究的进展这个引理越发吸引了大家的眼球，又有了更多人 follow 这项工作。
<a id="sec5-1"></a>

#### 5.1 对称的局部引理

LLL 考虑的内容是，如果有一些糟糕的事件 $$A_1, A_2, \dots, A_n$$，例如女孩子买一个包包，糟糕的事就可能是包的颜色不好看、包的质量不好、包的带子太短、包的容量不够、包的设计档次不行等等。我们希望能让这些事件统统不发生，按照概率方法的主要思想，就是要证明 $$\Pr(\cap_i \overline{A_i}) > 0$$，这样就能买到一个完美的包包。

然而，这种好事不会无条件成立。当然，对于极端的情况，比如所有事件独立，或者所有事件概率之和小于1，那我们很容易就能证明存在性。但是，如果这些事件不是完全独立，而是“几乎”独立呢？LLL 则给出了一组条件。

局部引理构造了这些事件的一个所谓 dependency graph（依赖图）。每个点代表一个事件。每个点与它的非邻居都独立，只跟它的邻居相关。这里需要强调的是此处的独立是 mutual independence，一个点 i 与邻居可能两两独立，但与某些邻居的一种组合相关，则它们也需要建立一条边。不过有很多应用场景不需要考虑这点，此时只要两两独立就一定互相独立。

在这个图上，LLL 的内容非常简洁：假设每个事件概率都小于某个值 p，并且每个事件对应节点的度都小于一个最大值 d，那么只要 $$4pd < 1$$，就一定成立 $$\Pr(\cap_i \overline{A_i}) > 0$$。直观上理解，就是每个事件概率别太大，相互之间牵扯别太多，那就一定存在同时避免这些坏事件的可能。这里对每个节点的要求都相同：同样的概率上界 p、同样的最大度 d，所以叫做对称的局部引理。

这个引理的证明有一些 trick，我们想要的形式是一堆事件的交的概率，一个常用的处理方法是把它展开成一串条件概率的乘积（类似做自然语言处理时对建模句子输出的常见套路，展开为语言模型）。但是当引入了条件概率时，我们必须保证充当条件的事件概率非零，否则没有定义。具体来说，我们要证明下式右侧每一项都大于0，以及每一个条件事件的概率也大于0。

$$
\Pr(\cap_i \overline{A_i})
  = \prod_i \Pr(\overline{A_i} \mid \cap_{j=1}^{i-1} \overline{A_j})
$$

于是 LLL 的证明就转化为证明：对于任意的非负整数 t，以及事件 $$A, B_1, B_2, \dots, B_t$$，求证下面两条：

1. $$\Pr(\cap_{j=1}^t \overline{B_j}) > 0$$；
2. $$\Pr(A\mid \cap_{j=1}^t \overline{B_j}) \le 2p$$。

这里的 2p 并不是一个紧的界，只是随便凑的，但为了证明 LLL 已经足够。

用归纳法，基础情形是当 t = 0 时，由于已知所有事件概率都小于 p，故两个不等式都直接成立。

当已经证明了所有 t' < t 时成立，对 t 的第1条，可同时用已证的两条：

$$
\Pr(\cap_{j=1}^t \overline{B_j})
 = \Pr(\cap_{j=1}^{t'} \overline{B_j})\Pr(\overline{B_t}\mid \cap_{j=1}^{t'} \overline{B_j})
 > 0
$$

t 时的第2条则稍微复杂一些。记 C 为所有 B 中 A 相关的那些事件，D 为与 A 不相关的那些 B 中的事件。具体来说，A 及其在依赖图中的所有邻居记作 $$\Gamma(A)$$，那么 C 集合就是 $$\{C_1, \dots, C_x \}= \{B_1, \dots, B_t\}\cap \Gamma(A)$$，D 集合则是 $$\{D_1, \dots, D_y \}= \{B_1, \dots, B_t\}\backslash \Gamma(A)$$。显然，两个集合的大小满足：$$x \le d, x + y = t$$。

如果 C 为空集时，说明 B 与 A 相互独立，那么第2条就直接得到。否则，

$$
\begin{align}
\Pr(A\mid \bigcap_{j=1}^t \overline{B_j})
    &= \frac{ \Pr(A\bigcap(\bigcap_{j=1}^t \overline{B_j})) }
            { \Pr(\bigcap_{j=1}^t \overline{B_j}) } \\
    &\le \frac  { \Pr(A\bigcap(\bigcap_{j=1}^t \overline{D_j})) }
                { \Pr((\bigcap_{j=1}^x \overline{C_j})\bigcap (\bigcap_{j=1}^y \overline{D_j})) } \\
    &= \frac    { \Pr(A\mid \bigcap_{j=1}^t \overline{D_j}) }
                { \Pr(\bigcap_{j=1}^x \overline{C_j} \mid \bigcap_{j=1}^y \overline{D_j}) } \\
    &= \frac    { \Pr(A) }
                { 1 - \Pr(\bigcup_{j=1}^x C_j \mid \bigcap_{j=1}^y \overline{D_j}) } \\
    &\le \frac  { p }
                { 1 - d/(2d) } \\
    &= 2p
\end{align}
$$

这个推导可能需要一些解释。第2步非常大胆地把与 A 相关的事件扔掉了，理由是交集的事件少了，概率肯定会稍大一些。第3步利用链式法则展开条件概率，分子分母中约去了先验概率。第5步再次使用 union bound 不等式把事件并集的概率放大成概率的和，而展开的每个概率在讨论 t' < t 时已经证明过，是归纳法的已知条件。

至此上述两条就证明完毕。对于我们想要的 LLL 最终概率，并且有 $$4pd < 1$$，不难得到，

$$
\begin{align}
\Pr(\cap_i \overline{A_i})
    &= \prod_i \Pr(\overline{A_i} \mid \cap_{j=1}^{i-1} \overline{A_j}) \\
    &= \prod_i (1 - \Pr(A_i \mid \cap_{j=1}^{i-1} \overline{A_j})) \\
    &\ge \prod_i (1 - 2p) > 0
\end{align}
$$

**应用实例**

作为一个简单的例子，我们考虑一个超图（即每条边可以连接2个以上的节点），每条边最多连接 r 个节点，并且会和最多 d 条边相交。如果 $$d \le 2^{r-3}$$，则该超图可以被 2-染色，即可以对每个顶点用两种颜色染色，使得每条超边都不是单色的。

假设我们随机独立地对每个顶点进行染色，由于每条超边最多有 r 个节点，所以一个超边是单色的概率为

$$
\Pr(A_i) \le 2\cdot(1/2)^r = 2^{1 - r}
$$

再考虑“一个超边是单色的”这一事件的不相关事件数目，显然已经知道它最多连接 d 条边，于是每个单色超边事件在依赖图上的度最多为 d，我们有

$$
4d\Pr(A_i) \le 4d \cdot 2^{1-r} \le 4\cdot 2^{r-3} \cdot 2^{1-r} = 1
$$

根据局部引理，完成证明。

<a id="sec5-2"></a>

#### 5.2 不对称的局部引理

局部引理是一个很有生命力的研究方向，提出后的几十年内，有了很多结果。首先就是几个不对称版本。

**(Spencer, 1975)** 还是在那个依赖图上，如果 $$\forall i, \sum_{j\in\Gamma(A_i)} \Pr(A_j) < \frac{1}{4}$$，那么 $$\Pr(\cap_i \overline{A_i}) > 0$$。

这个版本可以和对称的局部引理做直接对比，只要每个事件和它的邻居的概率之和小于 1/4，就能得到结论。与前面的 4pd < 1 的条件非常相似，并且体现出了局部引理中“局部”的特点：即只考虑一个事件和它的邻居，不用管别的。所以也是一个非对称的版本。

**(Spencer, 1977)** 然后又提出了一个更一般的非对称局部引理，如果能对找到 n 个对应每个事件的赋值，让每个事件的概率被这组赋值覆盖住，就能成立。具体来说就是，$$\exists x_1, \dots, x_n \in (0, 1), \forall i, \Pr(A_i) \le x_i \prod_{j\in\Gamma(A_i)} (1 - x_j)$$。

这些局部引理的证明都与对称版本类似，但稍微有一些变化，本文比较浅显就不列出了。

**(Shearer, 1985)** 提出了一个紧界，如果我们对这些事件所知道的信息没有更多了，局部引理要成立所需满足的紧条件的对称和非对称版本如下：

对称情形下，$$ \Pr(\cap \overline{A_i}) > 0 $$ 的充分条件为

$$
p < \begin{cases}
\frac{(d - 1)^{d - 1}}{d^d} &, d > 1 \\
\frac{1}{2} &, d = 1
\end{cases}
$$

能得到一个比较紧的推论：$$edp \le 1 \Rightarrow \Pr(\cap \overline{A_i}) > 0$$.

不对称的紧界需要引入记号 $$S \in ind(G)$$ 表示 S 是图 G 的一个独立集，即 S 中任意两点在 G 里都没有边直接相连。则 LLL 可以叙述如下，

$$
\forall S \subseteq T, \sum_{T\in ind(G)} (-1)^{\vert T\backslash S\vert} \prod_{i\in T} p_i > 0
\Rightarrow \Pr(\cap \overline{A_i}) > 0
$$

> 这一节写得太过粗糙，欢迎各位朋友来拍砖。

<a id="sec5-3"></a>
#### 5.3 构造算法

局部引理和其他概率方法相似，只给出了存在性证明，而没有构造性证明。然而其他概率方法可以容易导出比较直接的构造算法，局部引理却不能。主要原因还是局部引理算出的概率太小了，不太可能随机采样得到，另外还由于局部引理的一般形式比较繁琐，不太好设计算法。

**(Moser and Tardos, 2009)** 目前我们所知的最先进算法是本世纪才提出的 Moser-Tardos 算法。这个算法有一个要求：**每个坏事件都是由一组独立的随机变量生成**。这个条件看起来比较莫名，为什么要有它呢？但去掉这个条件的构造性算法还是一个开放性问题。虽然它让我们不爽，但是只要这个问题是在上面给出的紧界内的，这个算法都能很快地跑出结果来。而碰巧，目前大量用局部引理解决的问题都满足算法的这个要求。

并且这个算法真的非常简单，可以一句话描述：首先给所有的随机变量随机赋值，如果有某个坏事件 A 发生了，给那几个决定 A 的随机变量重新随机赋值，直到没有任何坏事件发生为止。

```cpp
for (X in all X's) {
    v[x] = a random value of X;
}

while (some A's occured) {
    arbitrarily pick an occured event A;
    for (X in A's determinant variables) {
        v[x] = a random value of X;
    }
}

return v;
```
<a id="sec6"></a>

### 6. 小结

本文总结了很多种用概率来证明存在性以及设计算法的套路。这些套路之间有一定的渐进关系：

- 计数。计算古典概率时，往往会用 union bound 不等式展开，这时我们实际上是展开以后单独处理每个随机变量或事件，本质上我们会认为它们完全相互独立。
- 一阶矩方法。我们往往会利用期望的线性性质，无差别地展开以后挨个求期望，这时不太强求这些事件到底是否相互独立。
- 二阶矩方法。算方差时往往避免不了随机变量两两之间的相关性，比如求协方差等。
- 局部引理。在所有的局部引理方法中我们都构造了一个依赖图，这个图事实上给出了这些事件的全局相关性，所有的相关和不相关我们都已知，再针对每个事件做局部的概率计算。

由此可见，局部引理考虑了最多的相关性，不难理解为什么使用局部引理能得到比计数法更好的界。

### 参考资料

本文基于教材和上课内容写成，非常感谢刘兴武老师精心准备的课程内容。这里给出一些参考资料，方便有兴趣的朋友进一步深入研究。

1. Mitzenmacher and Upfal. Probability and Computing (2nd). 2017（课程教科书）
2. Alon and Spencer. The Probabilistic Method (2nd). 2000（课程参考书）
3. Motwani and Raghavan. Randomized Algorithms. 1995.
4. 南大高级算法[课程网站](http://tcs.nju.edu.cn/wiki/index.php/%E9%AB%98%E7%BA%A7%E7%AE%97%E6%B3%95_%28Fall_2018%29)
5. Spencer. Ramsey's theorem-A new lower bound. 1975（本文正文中第一个不对称局部引理）
6. Spencer. Asymptotic lower bounds for Ramsey functions. 1977（本文正文中更一般的不对称局部引理）
7. Shearer. On a Problem of Spencer. 1985（局部引理的紧界）
8. Moser and Tardos. A constructive proof of the general Lovasz Local Lemma. 2009（局部引理的构造性算法）
9. Polipaka and Szegidy. Moser and Tardos Meet Lovász. 2011（局部引理的更多证明）
