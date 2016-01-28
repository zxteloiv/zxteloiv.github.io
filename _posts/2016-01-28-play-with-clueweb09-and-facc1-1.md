---
layout: post
title:  "ClueWeb09 数据集把玩日记（上）"
date:   2016-01-28 21:06
tags: learning ClueWeb09 dataset 
categories: chn learning
math_support: mathjax
---

作为一月份开始的一些工作记录，写下来备忘。发现写太长了，先把数据集文件部分放上来，后面继续（

### ClueWeb09数据集

#### 介绍

这是09年爬虫两个月内爬取的网页数据。它的子集已经用于很多工作当中。全集被用两个 3T 的硬盘保存并邮寄过来。压缩后的内容大约 5T。详细的情况可以查看[官方网站](http://www.lemurproject.org/clueweb09.php/)。		

> 例如英文的就在第一块硬盘中建立了 ClueWeb09_English_1 到ClueWeb09_English_10 10 个文件夹，全部包括了 en0000 ~ en0133 和 enwp00 ~ enwp03 共 137 个子文件夹，每个子文件夹下至多共有 00.warc.gz ~ 99.warc.gz 共计 100 个文件。

同时也有人在 ClueWeb09 之上做了相关的工作产出了一些相关的数据，这次我用到的 [FACC1](http://lemurproject.org/clueweb09/FACC1/) 就是其一。FACC1 在原网页的基础上标注出了网页中的大量实体词，并标注了对应的 [Freebase](https://www.freebase.com/) 节点。

> FACC1 的标注只有英文，有 ClueWeb09_English_1.tgz 到 ClueWeb09_English_10.tgz 对应原数据中的每个大文件夹，解压后分别对应到每个 00~99.warc.gz 有对应的 00~99.anns.tsv

#### 数据集文件格式

ClueWeb09 网页数据使用 [WARC 格式](http://www.digitalpreservation.gov/formats/fdd/fdd000236.shtml)保存。WARC 格式规定了一种保存网页数据的方法，但 ClueWeb09 并[没有严格遵循](http://lintool.github.io/Cloud9/docs/content/clue.html#malformed)它。主要是真实 URL 中可能包含一些乱码造成了存储的换行，其次有一些换行、头标签等的小问题。好在这些问题并不影响我们解析。

ClueWeb09 官网有一些样例文件，但仅做演示参考，实际上它的 Content-Length 计算有错误，是不能实际用来解析的。具体 WARC 格式的 header 可以参考标准文档，对 ClueWeb09 来说每个文件都有一个 warcinfo 类型 record 开头，后面都是 response 类型的 record，包含了 HTML 的返回结果：返回的头信息、返回的HTML 内容。下面删除了一些内容，加了原文不存在的注释以示说明。

> // WARC 头，第一个 record 为 warcinfo 类型
> 
> WARC/0.18
> 
> WARC-Type: warcinfo
> 
> Content-Type: application/warc-fields
> 
> Content-Length: 219
> 
> 
> 
> // WARC Record 内容
> 
> isPartOf: clueweb09-en
> 
> description: clueweb09 crawl with WARC output
> 
> format: WARC file version 0.18
> 
> conformsTo: http://www.archive.org/documents/WarcFileFormat-0.18.html
> 
> 
> 
> // WARC 头，后续都是 response 类型，WARC-TREC-ID 比较有用，以路径和编号唯一标识了一个网页
> 
> WARC/0.18
> 
> WARC-Type: response
> 
> WARC-TREC-ID: clueweb09-en0039-05-00000
> 
> Content-Type: application/http;msgtype=response
> 
> Content-Length: 24553
> 
> 
> 
> // HTML 内容
> 
> Content-Type: text/html; charset=UTF-8
> 
> X-Powered-By: PHP/5.2.8
> 
> Expires: Thu, 19 Nov 1981 08:52:00 GMT
> 
> (... html headers ...)
> 
> Set-Cookie: PHPSESSID=e992c851314bca6f032aedf8b5d75510; path=/
> 
> Content-Length: 24005
> 
> 
> 
> .... html raw content ....

FACC1 的文件格式则都是 tab 分割的纯文本，每行一条标注数据，包括了：

- 实体所在网页的 WARC-TREC-ID
- 实体文本、在 HTML 内容中的开始字节、结束字节
- 实体的准确性概率，有包含实体上下文和实体本身、不包含实体上下文两个值
- Freebase 路径，如 https://www.freebase.com/m/03bwzr4 对应路径就是 /m/03bwzr4

### 用 Python 读取数据

#### 源数据压缩方法

tar (Tape ARchive) 是 POSIX 标准之一的内容，没有找到免费的标准文本，可以参考 [wikipedia 上的说明](https://en.wikipedia.org/wiki/Tar_(computing))。它实现了把多个文件和文件夹进行压缩成一个文件的办法（这样在磁带机上写入会比较省时）。打包后每个文件称为一个 tar 中的 member。

[gzip](https://en.wikipedia.org/wiki/Gzip) 是一个文件格式，有对应的 [RFC 标准](https://tools.ietf.org/html/rfc1952)，里面只规定了文件头、校验信息等等，不限制压缩方法，但现在只使用 DEFLATE 压缩（即 LZ77 和 Huffman 编码）。虽然 gzip 可以支持多个文件压缩到一起，但是通行的做法依然是把多个文件打成 tar 包再做 gzip 压缩。相比 ZIP 文件来说这样可以利用多个文件的信息一起压缩，而 ZIP 压缩是分别把每个文件压缩后再合起来。zlib stream ([RFC 标准](https://tools.ietf.org/html/rfc1950#section-2.1)) 是另一种格式规范。而多个 gzip 合在一起仍然是合法的 gzip。

对 ClueWeb09 来说，它每个文件夹下的最终的文件都是纯文本的 WARC 经 gzip 压缩后的。而 FACC1 则是每个对应文件夹经过 tar 打包再由 gzip 压缩的。

#### 解析过程

由于原始数据非常大，基本不可能考虑在硬盘上解压后再处理。

对于 ClueWeb09 WARC 格式的文件有了解析工具：https://github.com/cdegroc/warc-clueweb，由于它考虑了上面提到的格式错误，并且使用内置的 GzipFile 模块，对 gzip 文件对象做处理，因此对 warc 和 warc.gz 文件都可以进行透明处理。源代码不多，可以简单读一下。经过这些包装以后，使用起来非常简单：

~~~python
import warc
f = warc.open("some.warc[.gz]", "rb")

# 使用迭代器简单处理
for record in f:
  pass

# 不能使用迭代器的情况：
record = f.read_record()
while record is not None:
  record = f.read_record()
  pass

print record # 全都打出来
print record.header # 打印出 record warc header
print record.payload # HTML 内容(包含 HTML 头信息)
~~~

在自己处理 Gzip 的过程中，可以参考 warc 用 GzipFile 模块的代码作为样例。

FACC1 使用的是 tar.gz 方式压缩，因此光使用 GzipFile 解压还不够，还需要考虑 tar 包的解压。然而 Python 内置的 tarfile 模块也支持了直接读取 tgz 的文档，打开时使用 "r" 是打开 tar 文件，使用 "r:gz" 就可以打开 tar.gz 文件。因此处理起来都非常简单。

TarFile 对象遍历迭代器得到的是 TarInfo 对象，对应一个 member，可以直接调用模块解压在当前路径或给定路径（就像在命令行用 tar 工具一样），但是在处理时我们需要操作文件读取内容，不能直接解压在硬盘上，因此需要用 TarFile.extractfile()

~~~python
import tarfile

tar = tarfile.open("some.tar[.gz]", "r[:gz]")
for member in tar:
  print member.name, member.size, member.isfile(), member.isdir()
  fileobj = tar.extractfile(member)
  line = fileobj.readline() # 从压缩后的一个文件中读取一行
  pass
~~~
