---
layout: post
title:  "五月工作小结"
date:   2016-05-31 18:00 +0800
tags: learning python attention 
categories: chn learning
math_support: 
---

又是一个月没有实质进展，感觉药丸啊

### 读书

**The Penguin Dictionary of Mathematics** Nelson, David   
从 H 港带回来的数学手册，事实证明没什么用，内容太浅或者不全或者不好查到

**银河帝国2-6** 艾萨克·阿西莫夫  
只有第一本和第三本的谋略部分比较好，其他几本勉强坚持看完

**2018 : 人类1月4日的逆袭** 刘慈欣  
短篇集气度不够，好评的没有几篇，比三体差太远了...

**The Princeton Companion to Mathematics**  
买了三年的普林斯顿数学指南，发现需要用到的时候还是很给力的

没意思的书

- 白夜, 文豪都是这么矫情的吗，想象力部分也不有趣

### 工作

百科爬虫:

- 搭建了一个爬虫，然而问题多多
    - 抓失败的有记录、以及暂停重启，之后如何迅速恢复到之前状态？
    - 需要直接从本地验证是否读取成功，但不知道中间件该加在哪，框架太大了虽然省力但还是看不懂
- 学了下 Go 放弃了，重新用 tornado 搭了镜像

wikipedia 事件和实体抽取标记:

- 补全了正例句子中的负例实体
- 加入了对数据集按事件类别区分的统计，用以验证 candidate key 的选择方法无误
- 做发布数据集的准备

docomo QA 系统:

- 开坑 OpenResty
- 读原版 JSP 代码

模式识别课的作业：

- 实现了一个 k-means 和 谱聚类: [Repo](https://github.com/zxteloiv/julia-ex/tree/master/clustering)

滴滴算法大赛:

- 没找到队友，第一次提交排名400多，希望后面能继续过初赛
- 实际上提供的数据很有限，比公司里简单多了

### 学习

正常向：

- 读了 10 来篇 QA 方法 paper 及综述，写了点[笔记](http://libzx.so/wiki/nlp/Survey_Question_Answering_Papers/)
- 参加了 R 大会，小老板的内容作了比较多的记录，感觉有了点收获，记录的[笔记](http://libzx.so/wiki/nlp/Lecture_R_lang/) 因为后面官方会有 slides 提供，所以就没记太多东西了
    - 近距离听了 rickjin 和 yiwang 的报告感觉好棒！
    - 提问被工作人员拍了个特写放到公众号的文章里感觉也不错（
- 终于把深度网络的入门两篇东西写了个总结，[back propagation](http://libzx.so/note/learning/2016/05/12/yet-another-tutorial-for-back-propagation.html) 和 [NN impl.](http://libzx.so/note/learning/2016/05/22/implement-a-neural-network-from-scratch.html)
    - 发现自己写起来完全也讲不清楚
- 读了一堆 SVM 的东西记了个[框架](http://libzx.so/wiki/pattern_recognition/Ch9_Support_Vector_Machine/) 后面慢慢补吧，不过 pluskid 大大的[系列笔记](http://blog.pluskid.org/?page_id=683)还是很好的，后面可以针对性地读
- 读了一大堆聚类相关的理论，感觉学到很多很开心，[笔记](http://libzx.so/chn/learning/2016/05/16/an-introduction-to-clustering.html)


休闲向:

- 之前没听过协变和逆变，写 julia 代码时有点困惑，然后在推上问了一下收到一大堆指点
    - wikipedia 页面介绍得比较简单: https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)
    - 一个形象的蔬菜和菜谱的例子：http://blog.ezyang.com/2014/11/tomatoes-are-a-subtype-of-vegetables/
- [How PNG Works](https://medium.com/@duhroach/how-png-works-f1174e3cc7b7#.8y99jhmfp) 回顾本科时的多媒体和数据压缩课...
- [为什么](https://howtotalkaboutarthistory.wordpress.com/2015/08/30/why-do-all-old-statues-have-such-small-penises/)古代的雕像里的人JJ都比较小呢（
- 在工业界和学术界写代码究竟有多少[相同和不同](https://da-data.blogspot.com/2016/04/stealing-googles-coding-practices-for.html) 这个 blog 非常受用
    - code review 依然非常重要，但可能 pair programming 也更好，但是 CR 过程中的一些具体操作也是要小心的，有可能起不到效果，比如必须让对方也熟悉项目、而且要提早CR等等
    - github commit 是一个很好的 CR 方式，可以选择在 merge 到 master 之前进行 commit 的 CR，同时在多人合作一个工作的时候 CR 也更有用
    - 但是没必要追求太多
    - 但是尽量开源，it's your job to be eaten (as a researcher)，所以尽量提供给别人你的资料，帮助别人做得更好
- [Go slices usage and internals](https://blog.golang.org/go-slices-usage-and-internals) 虽然不再写 Go 了，这篇文章算是给 py 写多的人看看到底 slice 可以怎么实现

### 生活

文明 5 是大坑啊！千万不要随便跳啊！当连续两天早上起不来以后就吓得我赶快删掉了（

好想玩群星啊，什么时候打折啊....

顺便过了个生日，好久没吃蛋糕了，虽然没约到人吃饭，不过自己过得也挺好。

虽然目前还要小心翼翼不被情绪困扰，但已经好转很多了。不被苦恋束缚、前方一片自由的感觉真的很好，不再为了想见她而去努力，而是自己单纯想要变得更强。

人生最重要的就是开心。

