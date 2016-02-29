---
layout: post
title:  "二月工作小结"
date:   2016-02-28 20:35
tags: learning corenlp java python debug 
categories: chn learning
math_support: 
---

二月由于春节假期，打乱了一下节奏，也没有干什么别的，主要就是三件事。  
一个，就是写了一个脚本用的框架。  
第二，就是了解了 java 组织和编译代码的方式。  
第三个，就是总结了一些 python debug 的经验。  
如果说还有一个那就是研究了 LCS 和 diff，这个对于调试 CoreNLP Server 有很大的关系，还有肉眼看出编码也是很大的。  
但那些都是次要的，主要就是三件事。很惭愧，做了点微小的工作，谢谢大家。

### 脚本框架“许仙”

写派森脚本时，如前篇[ClueWeb09 数据集把玩日记（下）](http://libzx.so/chn/learning/2016/01/29/play-with-clueweb09-and-facc1-2.html)最后所述，脚本的健壮性和日志的多样性往往会在日复一日的写脚本过程中消耗不少精力。基于以往经验，这种事情如果不能彻底解决，将后患无穷，消耗时间不说，还将消磨人的斗志。于是在写 ClueWeb09 数据集处理脚本的同时，重新考虑了之前自己一直困扰的几个点，力图以框架形式为脚本提供一个简明快的解决办法。框架的代码组织参考了一点 tornado 4.1 的代码，并吸取了以前应对需求的经验教训，例如 log rotation 这样简单的事情，tornado 原始的支持却非常死板，无法支持自定义格式和文件名，往往给后续日志收集与 ETL 带来麻烦。

许仙的代码仓库见：https://github.com/zxteloiv/py-xuxian

如 ReadMe 文件所说，脚本执行至少会遇到四种常见要求：健壮与失败重启、并行化加速、日志收集和产出、监控。许仙框架也想从这里出发解决这些矛盾。而现在的实现中虽然有效，但却比较初级。

#### 健壮性

最常见的情形就是真实数据中的各种脏乱导致代码出错。而人力处理也是费力不讨好。因此框架应该考虑如何处理这样的情形。最方便的办法是出错后自动拉起，对服务而言，用 supervisor 或者百度内部的 supervised 之类的工具非常方便，哪怕 `while true; do myservice; done` 的 shell 脚本也很可靠。然而对脚本而言，往往执行了一个巨大文件，不希望自动重启后从头开始。因此框架需要提供记录状态的功能。`xuxian.remember()` 和 `xuxian.recall()` 这一组 API 就是想做这个事。目前直接用写文件的方式来做。

然而在大量脚本执行的过程中还有一个常见场景是一次执行中先忽略出错的部分，打出 log 来，之后再单独针对出错的这部分重新跑一遍。现在只能使用脚本命令单独操作 log，许仙暂时没有支持。目前也没有想好要不要支持、怎么支持。

#### 并行化加速

对文件处理型的脚本，如果不涉及模型训练之类共享参数的情况，往往可以利用 data parallelism 进行多进程并行化。之前做 ClueWeb09 的经验是不要用 python multiprocessing module 管理多进程，然而有个 Wikipedia 的抽取工具 WikiExtractor 也利用了 multiprocessing 的 Pool 等，这块还没有找到一个好的实践方法。

后来处理 wikipedia 数据的时候，还是单线程使用许仙框架，同时使用 shell 来多进程执行。这样的执行非常稳定，能停掉单个进程并重启不影响其他的。

然而还有一个问题就是共享数据的服务化，一个 dict 的开销非常大，还是将简单的 map 结构放到 redis 里，而不是 python object，这样[更加节省资源](http://stackoverflow.com/questions/10264874/python-reducing-memory-usage-of-dictionary)。

#### 日志收集监控

之前不太懂 logging 的原理，后来读了一点 logging module 文档说明好了一些，以及 log_level 和 handler 的设计等。于是这里就直接包了一层，系统日志用 `xuxian.log.system_logger` 对象打到日志文件中，如果业务日志有需求的话可以单独申请 logger。程序的中间 dump、需要的监控信息等都可以从此接口输出。

### java 代码组织

做这个事情是被 CoreNLP Server 逼的。公开的 3.6.0 版本会有一些编码的问题（把输入的 UTF-8 视为 ISO-8859-1 强制又转成了 UTF-8），导致输出的时候编码错误，使得 token 分割和依存句法分析都变得不可用了。大概有三种办法来解决：

1. 使用 Protobuf 来与 Server 交互，由于还要重新捡起 pb 来研究一下代码怎么写，就放弃了。
2. 使用 diff 的思路，对其原始正确句子和返回错误编码句子，对齐无编码错误的英文部分，调整 token 和 mention 的 offset，然而实在太麻烦（用 LCS 找到最大子串并递归执行左右两边）
3. 搜了一下官方 repo 并且在 twitter 上收到了回复，确认最新版修复了此问题。

于是痛苦地把整个 repo clone 下来（nohup 然后直接地铁回家，不忍直视），直接参考[官方编译说明](http://stanfordnlp.github.io/CoreNLP/files/basic-compiling.txt)，由于使用最新 repo 源码编译，并没有 MENIFEST，只需执行

~~~ bash
ant
cd classes ; jar -cf ../stanford-corenlp-latest.jar edu ; cd ..
~~~

即可得到一个最新的 jar 包，而所有 CoreNLP 的代码都在这里。别的依赖都在 redistribute 发布中有了，同时下载 GitHub repo readme 中提到的最新 models 得到文件 stanford-corenlp-models-current.jar，拷到 redistribute 文件夹下，并移走原来的对应 stanford-corenlp-3.6.0.jar 之类的文件即可。

似乎现在都不推荐设定 CLASSPATH 了，执行的时候 java -cp 即可设定目录，按照官方方式启动 CoreNLP Server 就可以了。

如果硬要对比，`java -mx4g -cp "*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer [-port 23333]` 这样的方式其实相当于 `PYTHONPATH=blabla; python -m` 这样的形式，应该说这种执行代码的搜索路径与文件系统路径隔离的方式还是比较现代的。

### python debug

使用了框架之后好多东西都不用担心了，然而最近才知道了 ipdb 这个东西，具体可以参考神贴 [Python Debugging Tools](https://blog.ionelmc.ro/2013/06/05/python-debugging-tools/).

有时候一个 bug 是用 debug 还是 log 解决都是凭直觉的，目前还没有总结出什么小窍门。但在调 wikipedia bug 的过程中，往往发现 mention 或 token 的 offset 不对，然后一步步反追。然而一旦有断点，pdb 和 ipdb 的效率瞬间慢到 1/4，于是 debug 的过程也是异常烦人。就算有框架支持的 recovery，每次都跑还是很烦。

然而多跑几次就能发现了，善用 log level，然后花点心思把主要的几个[数据结构输出出来](https://github.com/zxteloiv/wikipedia-scripts/blob/master/main.py#L100)，这样每次执行直接看 log，然后迅速找到数据源头，发现果然有各种各样的脏数据没有考虑到，于是修改代码，之后换用 logging.INFO 跑。下次抛出异常了，继续用 logging.DEBUG 看看 log 什么情况。最终所有异常全部解决。

当然啦，一方面是因为 pdb 中敲变量名也挺累的，另一方面还是加载实体库和 wikipedia 词条跳转数据都比较大于是 pdb 中特别费时，如果使用 redis 服务化不但省了空间，还省了 debug 这样重复执行的时间。


