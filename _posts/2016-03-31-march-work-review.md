---
layout: post
title:  "三月工作小结"
date:   2016-03-31 23:20 +0800
tags: learning python attention 
categories: life
math_support: 
---

三月结束得挺快，最近的工作没有什么营养，不过还是总结一下.

### 读书

**亲密关系:通往灵魂的桥梁**  [加] 克里斯多福·孟  
对正确处理人际关系还是很有帮助的，大部分细节内容都不记得了，希望以后逐渐学会：诚恳、多思考、不说气话

**金字塔原理 : 思考、写作和解决问题的逻辑** [美] 巴巴拉·明托  
怎么表达自己的观点？自上而下，同级之间的逻辑性等等，等需要时再重新读一遍了

**网页排名PR值及其他 : 搜索引擎排序的科学** [美]兰维尔、[美]梅耶 / 郭斯羽  
PageRank 和 Markov Process 联系第一次知道，挺有意思的. 不过学了矩阵论和随机过程之后也就比较容易了.

**无厘头面试题2.0 : 估测原来可以这么简单·用简单推算巧妙解决实际问题** Lawrence Weinstein Patricia Edward  
很多细节很有意思，估算可以快速解决一些难题，同时理清思路. 估算时用几何平均数会比较好

**控制论与科学方法论** 金观涛、华国凡  
古老的好书，不过上学期了解过控制及稳态以后收获不是很大了，第一次听说了突变理论，水沸腾的例子非常有趣，在一个高维拓扑上描述了水在什么时候会沸腾什么时候不会，非常漂亮，虽然不一定能推广到很多事物上.

其他不太有意思的：

- 理解脑 : 新的学习科学的诞生，科普无新意
- 互联网+ : 国家战略行动路线图，宏观策略不创业暂时用不着，了解了解也罢
- 超级智能：路线图、危险性与应对策略，科普无新意，耸人听闻

### 工作

蛋疼类：

- 给工具(为终端输出加上彩虹渐变)提了个 python2 兼容的 fix 第一次[被 merged](https://github.com/tehmaze/lolcat/commits?author=zxteloiv)，有点小兴奋
- 新建了 quiver 笔记到 jekyll 的导出，以后可以把笔记以 wiki 的形式推到 blog 来分享并且利用 blog 的完善公式和标准 kramdown 语法支持
- 许仙库的更新，并把之前的自动机轮子提出来新建了一个 repo
- 妄图学 Haskell，被打回原形

正常向：

- 梳理了一下利用 redis server 来做脚本间资源文件共享的相关方法，目前跑起来了效果还不错，性能损失相比并行化带来的提升可以忽略
- 把 Wikipedia 数据生成了每个句子和 freebase entity 的对应，并继续帮忙做一点事件抽取，未完成
- 把 [Sentiment Analysis: Mining Opinions, Sentiments, and Emotions](https://book.douban.com/subject/26358294/) 的负责章节翻译完成，准备润色以后交给老板

### 学习

正常向：

- 学了 [python virtualenv 的用法](http://libzx.so/main/learning/2016/03/13/best-practice-for-virtualenv-and-git-repos.html)，把自己担心的几个问题都解决了
- 读了模式识别上课教材的前面几章，梳理了一点资料，虽然感觉做题的时候还有点疑惑，但前面的大体还可以：
    - [贝叶斯估计概要复述](http://libzx.so/chn/learning/2016/03/17/bayesian-estimation-outline.html)
    - [从参数估计到线性判别函数](http://libzx.so/chn/learning/2016/03/27/from-estimator-to-linear-discriminant.html) 后续的理解会继续在这篇上修改
- 了解 [Attention 机制](http://libzx.so/main/learning/2016/03/04/attention-reparaphrase.html) 的部分技术细节，感觉已经知道想知道的了，对于图像方向的应用没有继续看
- 学了小鹤双拼，感觉还可以，虽然速度不见得提升多少，但是编辑和修改都从以前的以字母为单位变成了以声母韵母为单位，确实略微有种爽爽的感觉

休闲向：

- [拓扑为何？](http://mp.weixin.qq.com/s?__biz=MzAwNTA5NTYxOA%3D%3D&idx=1&mid=402524062&scene=0&sn=8c115221d60a270ebb69e9e59e4f969f) 科普读物
- [代数几何 Grothendieck](http://www.teachblog.net/poincare/archive/2007/07/30/6616.html) 代数几何方向的名人趣事，什么东西都要自己推导、了解新知识靠基友
- [SSH Agent Forwarding](http://www.unixwiz.net/techtips/ssh-agent-forwarding.html) 我倒是看懂了 SSH 工具做了什么，但是还是不懂怎么配置啊喂（
- [Introduction to Microservices](https://www.nginx.com/blog/introduction-to-microservices/) 很久前的文章，讲的还可以，算是对以前工业实践的一个交代，暂时用不着后续没看了
- [程序与证明](https://web.archive.org/web/20141024034853/http://www.soimort.org/posts/168) excellent! 最后一部分没看懂，不过程序即证明这部分看得人简直要尖叫
- [Introduction to Mean Shift](https://saravananthirumuruganathan.wordpress.com/2010/04/01/introduction-to-mean-shift-algorithm/) 所以和 K-Means 有啥区别吗（
- 去听了邵成的 Haskell 小活动，终于能够完整背诵 A monad is just a monoid on the category of endofunctor, what is the problem 这句话了
- [Functors, Applicatives, And Monads In Pictures](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html) 原来我对通马桶工具的用法理解不对，难怪不能理解 monad，算是一个不带代数概念的比较好的入门，然而不知道有什么用
- [5 ways to improve running form](http://www.newbalance.com/article?id=3871) 小步幅，高频率，耶，不过还是先试着走快点吧

### 生活

起床时间 6 点不能保证了，还是晚上睡太晚的问题...

星际 2 战役原来输过一次秘籍后面就全废了，服了暴雪爸爸了，目前重来中...

情绪有过几次爆炸，还被人批评了，不开心，以后还是闷着点少表达心里想法。可能我的做法大多数人都会骂傻或者觉得 low 爆，那就继续变强让自己中二得更有底气吧。  
生活就像打麻将，如果有命运之神存在的话，她也一定只会眷顾不断向前的人.

不过话虽如此，但是还是要想想怎么处理心理问题，准备学点心理学的东西，毕竟唯物主义的长项只是处理人与物质的问题，人和自己的问题还是得想想别的办法，不过我肯定不会去信教的。

