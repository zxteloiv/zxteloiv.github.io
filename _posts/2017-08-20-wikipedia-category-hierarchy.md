---
layout: post
title:  "从 dump 数据构建 Wikipedia 的 Category Hierarchy"
date:   2017-08-20 14:30:00 +0800
tags: wikipedia
categories: chn
math_support: 
---

整理 Wikipedia 数据花了一天，简单记一下提取类别体系构建上下位关系的一些问题。

# 背景

每一个 Wikipedia 页面下方都有一条 Category 信息，表示这个页面属于哪些类别。

这些分类的信息非常有价值。第一，它是众包的。类别词会有很多正文里没有的内容，并且是人工对页面进行的某种归纳。若用分类建立索引，可以实现很多正文匹配不了的检索（比如搜索“国家4A级景区”）。
第二，它某种意义上代表了这个页面对应的“类型”。不同于程序语言的类型，这些类别代表了在人类认识世界的广泛意义上一个实体的类别归属，对于构建知识图谱及相关应用非常有帮助，甚至在一定的小范围内可以使用类型理论的工具进行相关推导。
第三，当我们想限制要处理的 Wikipedia 数据，仅对某个领域做处理，这时就需要用 Category 进行过滤。
第四，这些类别是人工维护的。它可以时刻保持最新，并有人不停地修正里面的错误，其数据质量目前是自动化方法无法取代的。

# 数据来源

## 下载

Wikipedia 的 dump 数据大多可以从 [dumps.wikimedia.org](https://dumps.wikimedia.org/) 中得到。由于只关注文本方面的内容，因此页面的文件附件这里没有去研究过。除了一些年代久远的 dump 以外，最新的数据可以从很多方式获得：

- Database backup dumps: [link](https://dumps.wikimedia.org/backup-index.html) 页面下方有各种子站的链接，包括 Wikimedia 基金会下面的多个其他项目的数据（Wikidata、wikiquote、wikinews 等等）。点 enwiki/zhwiki 进入可以看到一个 dump  清单。由于 Wikipedia 文档非常多，一次 dump 要花很久完成，清单中有一些有用的 sql 文件是 wikipedia 自身数据库表的 dump，一些中间步骤的 xml dump，以及常用的全文 dump（如 enwiki-20170801-pages-articles-multistream.xml.bz2）
- Other files: 这里有一些奇奇怪怪的 dump，其中 CirrusSearch 的 dump 这次主要也用到了，相较 xml 的全文 dump 而言更为方便。

## 提取

类别本身也拥有对应的页面：比如[社会分类](https://zh.wikipedia.org/wiki/Category:社会)。在导出的文件中，除了普通页面会有所属的分类之外，类别的页面也会给出所属的分类。据此可以提取出类别树。

至少有三种手段可以从数据中提取出类别信息：

- 导出的 sql 文件中有 `category.sql` 和 `categorylinks.sql` 两张表。其中 [categorylinks 表](https://www.mediawiki.org/wiki/Manual:Categorylinks_table)提供了每个页面 id 到每个 category 的映射数据。为了获取文章标题还需要再使用 `page.sql` 表。用这种方法提取类别树实际上就是在类别自己的页面上找所属类别，具体解释可以参考[这篇 blog](https://kodingnotes.wordpress.com/2014/12/03/parsing-wikipedia-page-hierarchy/)
- 从 XML dump 中提取类别也是可行的。因为所有的页面归属的类别都写在 wiki 页面文本中，例如[武官](https://zh.wikipedia.org/wiki/%E6%AD%A6%E5%AE%98)的页面源数据下方就有 `\[\[Category:官职\]\]` 的写法。从而可以从正文中提取出所属类别，而利用类别页面自身的所属类别信息就能构建类别和类别之间的上下位关系。
- 从 CirrusSearch dump 中提取最为方便，由于它已经做了如模板展开等工作，所有数据存为 json，连续的两个 json 对象（一个 index 接着一个 content）可以得到关于页面的所有内容：所属 [namespace](https://www.mediawiki.org/wiki/Manual:Namespace) 、对应页面的热门打分（popularity_score）、页面所有的 Category、页面可以由哪些词重定向得到，以及正常的页面标题、正文、页面 id 等内容。

目前还比较能用的常见 Wikipedia 提取工具是 [WikiExtractor](https://github.com/attardi/wikiextractor)，然而它现在无法提取出对应的类别信息，只能提取出正文。因此推荐的方法是从 Cirrus Dump 中[自己写脚本](https://github.com/zxteloiv/wikipedia-scripts/blob/master/cirrus/filter_cirrus.py#L51)提取需要的字段。

## 注意事项

\1. CirrusSearch Dump 分为两种，一个是 xxx-content.json.gz，另一个是 xxx-general.json.gz。前者是正文数据（namespace=0，namespace 含义查看[这里](https://www.mediawiki.org/wiki/Manual:Namespace)），
后者是所有讨论、元数据等信息（namespace!=0）。因此，如果需要建立类别的 hierarchy 只能从 general dump 里获得。

\2. 重定向信息虽然与 Category 无关，但事实上是必需的。content dump 和 general dump 中都需要提取对应的重定向映射表。Category 也是可能有重定向的，一个旧的 Category 名称可能会被重定向到新的页面。

\3. 可以用 API 查询具体的 pageid 得到对应的 Wikipedia url：https://zh.wikipedia.org/w/api.php?action=query&prop=info&inprop=url&format=json&pageids=1668091

\4. 类别的归属、以及页面对于类别的归属，事实上并不是一个树结构，而是一个非常复杂的图，需要引入更多的先验知识才能把一些局部图结构拆解成树形。因此应该只考虑建立局部的两两之间上下位关系，不应该去试图虑建立全局的 Ontology。

\5. 对中文而言，不同的简繁体之间有所区别。因此会有一个页面属于某个 Category 但那个 Category 却找不到更多上位关系或者直接找不到自身的情况，这可能是由于该 Category 需要做简繁转换（在 zhwiki 上输入时会自动跳转），可以考虑使用 opencc 处理转换，并加入类别重定向来做。

# 进一步扩展

这里讨论的只是一个语言子站的类别，例如单独的 zhwiki 或 enwiki 都可以这么处理。

但是页面直接是可以互相链接的，中英日文的 Wikipedia 都可以互相跳转。于是：

页面本身可以跳转，例如中文 wiki 的[光合作用](https://zh.wikipedia.org/wiki/光合作用) 和英文的 [Photosynthesis](https://en.wikipedia.org/wiki/Photosynthesis) 及日文的 [光合成](https://ja.wikipedia.org/wiki/光合成) 之间是可以联系起来的，因此可以认为这三者会对应同一个东西，那么三者对应的 Category 也理应是可以互通的。中文光合作用可以对应到英文的 [Botany 类别](https://en.wikipedia.org/wiki/Category:Botany) 和 [Plant_nutrition 类别](https://en.wikipedia.org/wiki/Category:Plant_nutrition).

类别本身也是有页面的。因此不同语言的类别之间也可以建立对应的等价关系。例如 [Botany 类别](https://en.wikipedia.org/wiki/Category:Botany) 和[植物学类别](https://zh.wikipedia.org/wiki/Category:植物學)是等价的。如果一个中文页面缺失了某个中文类别，但英文页面有该类别的英文版，那可以认为这个中文页面也会对应到该中文类别之上。

这些对应关系不止可以双语，甚至还可以扩展到三语上（处理起来会稍微有些麻烦）。虽然一般认为英文 Wikipedia 是数据量最大从而最全的，但事实上确实有很多中文、日文的页面、类别是自己特有的，英文不具有。可以认为它应该具有一个类别，但是此类别可能只在日文里有一个现成的符号来描述，缺少中英的对应。
通过这样的多语链接，可以极大地扩展页面所属的类型项。


