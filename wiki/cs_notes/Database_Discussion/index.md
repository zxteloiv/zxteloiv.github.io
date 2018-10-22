---
layout: page
title: Database Discussion
math_support: mathjax
---


LevelDB (KV)
Berkeley DB 
Rocksdb (KV)

TerarkDB https://terark.com/en/engines/terark-db 
https://zhuanlan.zhihu.com/p/21493877
https://www.zhihu.com/question/46787984
https://www.usenix.org/system/files/conference/nsdi15/nsdi15-paper-agarwal.pdf


SSDB http://ssdb.io/zh_cn/ (redis+persistent)
SWAPDB https://github.com/JingchengLi/swapdb (redis+persistent)

Dgraph

Haruki Kirigaya (Lily), [29.08.17 19:20] 

有没有支持压缩的 db 呀…… 

 

Haruki Kirigaya (Lily), [29.08.17 19:20] 

我有一个 json 压缩包是 bz2 的，解压完 ~300G 不敢动，压完11G，但是既然 bz2 是 stream 也可以在上面实现一个 b-tree  吧 

 

Googol Lee, [29.08.17 19:32] 

很多db都支持压缩存储，但可能和你想的不一样 

 

Googol Lee, [29.08.17 19:32] 

要看bz2是不是支持随机寻址 

 

Googol Lee, [29.08.17 19:33] 

Stream是不要求随机寻址的，但是数据库一定要，不然就是磁带的速度 

 

Googol Lee, [29.08.17 19:36] 

如果你的数据只需要kv，你可以试试rocksdb 

 

Haruki Kirigaya (Lily), [29.08.17 19:38] 

我试试（ 

就记得有几个 leveldb 的替代一时搜不出来（ 

 

Googol Lee, [29.08.17 19:39] 

但是这几个只是紧凑存储，应该没压缩 

 

Googol Lee, [29.08.17 19:40] 

直接用leveldb也差不大 

 

羽神 桐山, [29.08.17 20:10] 

https://terark.com/zh/index 

 

Haruki Kirigaya (Lily), [29.08.17 20:12] 

[ Photo ] 

 

羽神 桐山, [29.08.17 20:12] 

球大佬测评呀 

 

Googol Lee, [29.08.17 20:13] 

最核心的 TerarkDB：基于 RocksDB，使用 Terark 核心技术的存储引擎 

 

Haruki Kirigaya (Lily), [29.08.17 20:14] 

等有空的（ 

正好上次那个 30M/s 就写死的破服务器可以用来测测 

 

Googol Lee, [29.08.17 20:14] 

我觉得就是在rocksdb上加了一层zip（ 

 

Haruki Kirigaya (Lily), [29.08.17 20:14] 

其实我希望它能直接用 mongo interface 兼容我下载的 bz2/gzip 

 

Haruki Kirigaya (Lily), [29.08.17 20:14] 

这样连导入都不需要了，扫一遍建好索引就开始干活 

 

Googol Lee, [29.08.17 20:15] 

这没可能，醒醒 

 

Haruki Kirigaya (Lily), [29.08.17 20:15] 

如果只要 readonly 呢（ 

 

Googol Lee, [29.08.17 20:15] 

先不说zip和bz2/gzip都不一样，即便同一种压缩，raw的存储格式也不可能一样 

 

Haruki Kirigaya (Lily), [29.08.17 20:16] 

可以存一下每个 object 的 offset 呀（ 

 

Haruki Kirigaya (Lily), [29.08.17 20:16] 

不过 bz2 最小块也是 100K 估计性能不行 

 

Googol Lee, [29.08.17 20:16] 

zip本身不支持random seek。现在要么是按行压缩要么是按列压缩 

 

Googol Lee, [29.08.17 20:16] 

还没有跨行/列压缩的 

 

Googol Lee, [29.08.17 20:16] 

如果你要极限压缩，一般列压缩比行压缩的压缩比高 

 

Haruki Kirigaya (Lily), [29.08.17 20:17] 

日常搞的文件肯定都是行式的…… 

 

Googol Lee, [29.08.17 20:17] 

你那个不叫行压缩，你那个叫全文压缩 

 

Haruki Kirigaya (Lily), [29.08.17 20:18] 

[ GIF ] 

 

Googol Lee, [29.08.17 20:18] 

行压缩需要能直接定位到某一行的压缩数据，然后直接解码 

 

Googol Lee, [29.08.17 20:18] 

全文压缩丢失所有行头信息 

 

Haruki Kirigaya (Lily), [29.08.17 20:18] 

我现在想的是 seek 到那里然后再解码那个块（ 

 

Googol Lee, [29.08.17 20:18] 

行头都找不到，怎么做seek 

 

Haruki Kirigaya (Lily), [29.08.17 20:19] 

索引存一个（ 

 

Googol Lee, [29.08.17 20:19] 

而且zip这种感知压缩，是需要前续解压生成的字典的 

 

Googol Lee, [29.08.17 20:19] 

不把前面都过一遍，光跳过去读数据，也不知道怎么解压 

 

Haruki Kirigaya (Lily), [29.08.17 20:20] 

等等，gzip 和 zip 不太一样吧？ 

 

Googol Lee, [29.08.17 20:21] 

差不多，底层算法都是DEFLATE或变种 

 

Googol Lee, [29.08.17 20:21] 

只不过封装格式有区别 

 

Googol Lee, [29.08.17 20:23] 

一般wiredtier这种都是数据按行分块压缩，然后对块做索引，而不是整个数据库统一压缩 

 

Haruki Kirigaya (Lily), [29.08.17 20:23] 

gzip 其实是分块了以后假装是流么（ 

zip 可以跳过个别文件 

那最差也能索引到对应 block 的位置然后解压整个 block 再顺序查到需要的 

按现在每个 object 是 1w bytes 的 json 来看的话block只要控制到 10k就可以 

 

Googol Lee, [29.08.17 20:23] 

你使用的这些压缩软件，其实都是打包软件，不是分块压缩，而是一个文件整体压缩打包 

 

Haruki Kirigaya (Lily), [29.08.17 20:24] 

这样…… 

 

Googol Lee, [29.08.17 20:24] 

这些软件内部的分块是为了打包优化，与数据本身没关系 

 

Googol Lee, [29.08.17 20:25] 

比如对字典的优化，毕竟字典太大了没办法装入内存会引起很大性能问题 

 

羽神 桐山, [29.08.17 20:26] 

[In reply to Googol Lee] 

没办法解释他们声称的性能提升（ 

 

Haruki Kirigaya (Lily), [29.08.17 20:26] 

我还以为是每个块存一个字典……单独压缩每个块 

 

Googol Lee, [29.08.17 20:26] 

压缩的话确实在读取方面会有性能提升，毕竟io总量变小了 

 

Googol Lee, [29.08.17 20:27] 

[In reply to Haruki Kirigaya (Lily)] 

确实是每个块一个字典，但你不能保证每个块都包含的是正行数据。切在一行中间的怎么办？ 

 

Haruki Kirigaya (Lily), [29.08.17 20:29] 

是这样……所以要存一个块的起始，然后连续搜1~3个块，找到想要的……大体上每个行是 10k~50k，不过这有点单独为 bz2 做个 wrapper 的意思 

 

Googol Lee, [29.08.17 20:30] 

看了一眼，terarkdb对key和value分开做压缩，用的不同算法。不知道适应性怎么样 

 

Googol Lee, [29.08.17 20:30] 

[In reply to Haruki Kirigaya (Lily)] 

bz2只是一个程序，不是压缩算法 

 

Haruki Kirigaya (Lily), [29.08.17 20:31] 

[ Photo ] 

可以的 

 

Haruki Kirigaya (Lily), [29.08.17 20:31] 

好亲切的地方（ 

 

Googol Lee, [29.08.17 20:31] 

你要不去拜访一下（ 

 

Googol Lee, [29.08.17 20:31] 

我估计是阿里的人出来搞的 

 

Googol Lee, [29.08.17 20:31] 

看客户列表里只有阿里 


