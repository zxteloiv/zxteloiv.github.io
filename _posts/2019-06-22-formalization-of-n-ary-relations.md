---
layout: post
title:  "N元关系的表示模型"
date:   2019-06-22 19:00 +0800
tags: n-ary representation_models RDF Semantic_Web Knowledge_Graph
categories: chn
math_support: mathjax
---

前做了个N元关系表示的调研，这篇文章对此概念做个简要的介绍。本文将首先说明知识图谱和语义网的一些背景，再介绍为什么要表示N元关系，最后介绍多个表示方法。

# 1. 背景

知识是人类以往经验的总结，无论是自然界的规律、亦或人类创造的工具、甚至人类社会自身的历史，都可以系统地组织起来。虽然这方面的研究源远流长，先驱可以追溯到上个世界中期，但谷歌的知识图谱是这方面第一个出名的工业产品，它有着以往研究中未曾处理过的巨大规模，是个从互联网文本中挖掘的知识宝藏。那么，你是否曾想过，这个东西如果用现代的数据库，应该怎样表示、怎样存下来？

我们容易发现，很多基本知识都涉及了两个东西的某个关系，并能用主谓宾的形式进行表达，例如“美国的总统是奥巴马”、“中国的首都是北京”、“唐代的最高权力机关是中书省”等，分别可以表达为“总统（美国，奥巴马）”、“首都（中国，北京）”、“最高权力机关（唐代，中书省）”。据此，人们在形式上把这样的知识用包含三个元素——即谓词、主语、宾语——的三元组来表示，而所有这些三元组的集合就形成了所谓知识库（Knowledge Base）的概念。

W3C最先定义了比较通行的[RDF规范](https://www.w3.org/TR/rdf-mt/)，具体规定了三元组应该以什么样的文本形式保存，文件格式和语法等。同时也定义了配套的类似SQL的查询语言：[SparQL](https://www.w3.org/TR/rdf-sparql-query/)，以便查询这样的RDF数据。如果使用[关系数据库做后端](http://vos.openlinksw.com/owiki/wiki/VOS)，三元组可以直接对应一张表的三列（当然为了实用，还需要进行分表、加入索引、添加元数据等工作）。容易理解，知识库可以简单地说成是已经有固定模式的数据库。而如今有越来越多的NoSQL或图数据库，换用它们也能获得很大的性能提升。后续的很多工作也可以基于这个三元组来构建。

三元组其实表示的是一种二元关系，陈述了两个实体概念之间的联系。当然我们可以质疑，如果某个知识只有一个实体怎么办？例如，“牛顿终生未婚”。实际上这并不妨碍我们用三元组的形式来保存它，例如在[WikiData的数据模型](https://www.mediawiki.org/wiki/Wikibase/DataModel/JSON)中，每个尾实体的数据类型被做了很复杂的类型提升，当尾实体类型（snaptype字段）为“somevalue”时对应上述通常的二元关系，而“novalue”表示此关系无值，即理解为“配偶（牛顿，null）”即可。所以三元组表示一元关系、二元关系都很容易，那么N元关系呢？

# 2. N元关系的必要性

顾名思义，N元关系的一个关系中可能有N个实体参与。这并不是故意复杂化，实际上这样的例子比比皆是。例如，“A公司持有B公司的股份，持股比例为10%”，“X有三段婚姻，对应的三位配偶分别是A、B、C”，“某国在2017年、2018年、2019年的GDP分别是多少”等等。试想，如果我们在数据库中用三元组记录如下内容：

| 谓词 | 主语 | 宾语 |
| --   | --   |  --  |
| 配偶 | X    | A    |
| 配偶 | X    | B    |
| 结婚时间 | X | 2010 |
| 结婚时间 | X | 2013 |

容易发现这个数据库是有歧义的，后续的查询将无法得知哪个结婚时间对应哪段婚姻关系。

还有一个较为复杂的例子，在语义解析的调研中曾用于介绍语义表示方法，它同样也阐明了相似的情形。给定句子: Brutus stabs Caesar，它表示Brutus刺杀了Caesar，那么它自然阐述了两个人之间的一种关系，照上面三元组的办法，我们依葫芦画瓢，形式地记作: stab(Brutus, Caesar)

- 句子加入更多内容: Brutus stabs Caesar with a knife. 我们如果抛弃三元组，强行加入更多参数，可以记作: stab(Brutus, Caesar, knife).
- 但如果换一下附加成分：Brutus stabs Caesar in the agora. 此时如果还用多元记法：stab(Brutus, Caesar, agora). 但这样的表示法与上面就产生了歧义。
- 如果两个附加成分同时出现：Brutus stabs Caesar with a knife in the agora. 实在没办法我们可以把每个附加修饰都移出为其他谓词：stab(Brutus, Caesar)^with(knife)^in(agora).

不幸的是，问题并未解决。第一是修饰歧义的问题。如果多有一个动词，例如：Brutus stabs Caesar with a knife in the agora and twist it hard. 如果表示为 stab(Brutus, Caesar) ^ with(knife) ^ in(agora) ^ twist(Brutus, knife) ^ hard，此时最后的 hard 究竟是修饰 stab 还是 twist 就无从得知。第二，在自然语言中，论元的数目并不是那么必要且固定的。例如，Pass the axe 和 Pass me the axe 都相近，且语义不容易有理解错误，那么要给谓词加多少个元才是个头？

语言哲学家 [Donald H. Davidson](https://en.wikipedia.org/wiki/Donald_Davidson_%28philosopher%29) 为我们提出了一种解决方法，但这种针对多元关系表示有多种模型，我们在下一节里分别介绍。

# 3. 多元关系的表示方法

上面已经介绍了一种最简单粗暴的表示方法：把多元关系拆解开，两两之间用三元组存储。但遗憾的是上面演示了这种方法会在知识库中引入歧义，无法使用。人们又发明了形形色色的多种办法。下面的介绍都用同一个虚构的事实为例：“John和Kathy在2010年结婚”。

## 3.1 子属性方法

第一种表示法称为子属性方法。多元关系中每两个实体都可能具有某个关系，但与上面不同的是，我们用一个多余的特殊的节点来表示它们的关系。而这个特殊的节点记作已知关系的子属性。所有的新增子属性之间我们再加入三元组，表示它们完全“等价”，即它们其实是在说一个事实，而不是多个。

这种方式用三元组形式来记录，可以写作如下表格：

| 谓词 | 主语 | 宾语 |
| --   | --   |  --  |
| V1 | John    | Kathy    |
| 子属性 | V1  | 结婚对象    |
| V2 | John | 2010 |
| 子属性 | V2 | 结婚日期 |
| V3 | Kathy | 2010 |
| 子属性 | V3 | 结婚日期 |
| 等价 | V1 | V2 |
| 等价 | V2 | V3 |
| 等价 | V2 | V1 |
| 等价 | V3 | V1 |
| 等价 | V3 | V2 |
| 等价 | V1 | V3 |

由于表格过于繁琐，后面的例子都不再列出表格。用图示来理解可能较为直观，上述三元组集合可以表示如下图：

<img height="250px"
     src="/assets/imgs/2019/0622-subproperty.png"
     alt="Subproperty"
/>

可以很直观地看出，这种方法将二元关系派生出子属性，仅在此事实中使用。同时这些特殊的子属性相互等价，可以看做是同一个特殊关系，多继承自好几个二元关系。

- 这种做法可以保证查询时的便利性，因为实际使用时大多数时候只需要验证两个实体之间是否有某种关系。
- 但是这种做法引入了大量的“等价”关系，由于是相互的，而三元组是有方向的，因此往往需要记录多倍的“等价”关系。这也可以从上面的表中看出来，因此这样的存储有些冗余。

## 3.2 RDF Reification 方法

Reification 在 RDF 规范中是一个定义好的术语，它指将某种语法归类为一个语义对象，并用另外的语法来描述这个对象。具体在本文所说的这个N元关系场景下，N元关系的表示就是一个语法现象，而我们将这个语法现象看做一个语义对象，再用别的语法来表示这个关系中有哪些元。

举例来说，“维基百科里说川普是美国总统”这个句子，当我们没有引入 Reification 时可能会比较困难表达“维基百科里说”这层意思，但可以很容易表达：“总统（美国，川普）”这个关系。
如果我们把这个句子所陈述的事实看做是特殊的实体“x”，那么这个事实用RDF规范可以表述成如下的三元组集合：

> x rdf:type rdf:Statement .

> x rdf:Subject <川普> .

> x rdf:Predicate <是总统> .

> x rdf:Object <美国> .

> x asserted_by  <维基百科> .

可以看到我们繁琐地用三元组表达了这个事实的主语、宾语、谓语分别是什么，同时也就可以加上“这个事实是由维基百科说的”这样的含义。著名的知识库YAGO2，通过这样的方式，单独扩充了原始的YAGO中的知识库，为很多三元组事实都加上了该事实的时空属性。例如YAGO2中，“姚明出生于上海”这一三元组就通过上述办法扩充了“出生时间是1980年”以及上海的经纬度坐标。

> id_truj59_oyl_o82iaz  &lt;occursSince&gt;  “1980-09-12” .

> id_truj59_oyl_o82iaz  &lt;occursUntil&gt;  “1980-09-12” .

> id_truj59_oyl_o82iaz  &lt;occursIn&gt;  &lt;Shanghai&gt; .

因此，如果使用了 RDF Reification，我们可以从元事实的角度上进行描述，从而方便做一些如推理、证明等任务。回到上面的例子，“John和Kathy在2010年结婚”可以用类似方法表示为下图：

<img height="250px"
     src="/assets/imgs/2019/0622-reification.png"
     alt="RDF Reification"
/>

但这种方法同样会遇到麻烦。表述多元关系的开销依然会随着“元”的增加而指数级增加，因为两两之间都需要加入双向“等价”关系，同时所有的二元关系都表示成了Reification。此外，元数据和事实会混淆在一起难以区分，例如“由维基百科说”这一事实和“这个三元组是一个Statement”二者就难以分别处理。

## 3.3 Neo-davidsonian 方法

刚才说到，D. H. Davidson 提出了一种多元关系的解决办法，即加入“事件”节点，而事件有很多组成成分。对于上面 Brutus 被 Caesar 刺杀的例子：Brutus stabs Caesar with a knife in the agora and twisted it hard. 在 Davidson 的观点下，就可以表达为存在两个事件：stab 事件和 twist 事件，写作 lambda 表达式的话就是：

> ∃e1.stab(e1, Brutus, Caesar) ∧ with(e1, knife) ∧ in(e1, agora) ∧ (∃e2 .twist(e2, Brutus, knife) ∧ hard(e2))

回到上面 John 和 Kathy 结婚的例子，我们可以定义一个婚姻事件，从而各个元都是该事件的组成成分。而 Freebase 中所引入的 CVT 节点、FRED系统中借用了 FrameNet 的框架，则都是用这一类方法来解决多元关系问题的。

<img height="250px"
     src="/assets/imgs/2019/0622-cvt.png"
     alt="CVT"
/>

细心的同学可能发现了，上面说的都是 Davidson 提出的也就是 Davidsonian 方法，那么 Neo- (新的) Davidsonian 究竟有啥新？

上面 stab(Brutus, Caesar) 和 twist(Brutus, knife) 两种是Davidsonian方法中所用的谓词表示，这么写的话所有的动词都需要我们特地标记两个参数分别是什么。但是这些参数之间并不是完全独立的，好像还是有那么一点相似性，比如好像都是A对B做了一点什么事的样子。为了捕捉这种跨动词之间存在的相似性，因此后人对Davidsonian加入了主题角色（Thematic Role）的概念，（大部分地）规范化了动词的参数之间是什么关系：

<img height="250px"
     src="/assets/imgs/2019/0622-thematic-role.png"
     alt="Thematic Roles (image from the book Speech and Language Processing (2nd) by D. Jurafsky and J. H. Martin)"
/>

因此，这里 stab、twist 的第一个参数都可以叫做 AGENT，第二个参数则都是 THEME 或称 PATIENT。而上面的例子图中写作 Participant 同样有这方面的考虑。然而语义角色并非一个定义良好的封闭集合，除了一些基本的角色之外尚有很多争议，本文对此按下不表。

## 3.4 FrameBase

确切地说 FrameBase 只是一个知识库，它的表示模型并没有在其他系统中采用。但它基于 Deo-Davidsonian 方法，并针对其问题而给出了自己的处理方式。

上面的“框架”式或者“事件”式解决办法只能将多元关系表示为一个比较复杂的结构体。但日常应用时大多数情形只需要验证两个实体是否有某种关系，而在Davidsonian表示模型下就很不方便了。为此，FrameBase 引入了自动化的规则手段，加入很多二元的联系（可能是冗余地生成二元关系进行存储，不太可能是在线的查询，这里我没有去验证）从而使得二元关系查询也变得很简洁。

此外 FrameBase 的每个 Frame 并不是像上面那样随意定义，而是采取了与 FrameNet 相似的方式，即规定了很多具体词汇用于触发此框架，例如 marriage, marry, wedding 等不同词性、词汇都可以表示结婚这个概念。这样与普通词汇进行结合的方式有助于将知识和语言相联系起来。

因此，上面的结婚例子可以用图示表示如下，绿色部分指规则生成的二元关系：

<img height="250px"
     src="/assets/imgs/2019/0622-framebase.png"
     alt="FrameBase"
/>

# 4. 表现形式

为了尽量保证背景知识的完整性，这里夹杂一些私货，具体想讨论一下语义知识到底有哪些表现手段。这里主要倾向于指形式语义，而不是现在比较流行的词向量、BERT等分布式表示方法，这二者的对立和融合可以参考论文[1]. 但由于对这方面的整体体系不太熟悉，所以这里只能琐碎地介绍一番语义的表示方法。

意义表示(meaning representation)是将语言的含义(meaning)表示成形式化结构[2]。在这些形式化表示方法的路途上人们有很多尝试，这里简单介绍一部分语义表示的形式化方法。这些方法一般都涉及两个方面，一个是本身代表的语义概念以及设计者对语义的理解，另一个则是具体这种形式方法应该怎样用符号在纸上写出来。

最开始，人们往往使用一阶逻辑的记号来写出形式语义。这可能是继承自逻辑、推理、形式语言等学科的习惯。而形式语义也在这样的有点像lambda表达式又有点像LISP但是主要用一阶逻辑符号的基础上进行讨论。例如用一阶逻辑写出如下两个句子的语义：

<img height="250px"
     src="/assets/imgs/2019/0622-first-order-logic.png"
     alt="First Order Logic"
/>

而一阶逻辑可以用如下的上下文无关语法来生成：

<img height="250px"
     src="/assets/imgs/2019/0622-grammar-for-fol.png"
     alt="First Order Logic Grammar"
/>

在后来的探索中，为了与句法结构相接近以及parsing的方便，Percy Liang等人提出了λDCS的表示方法。这里不讨论这样的形式，具体可以参考文献[3]，本文按下不表。

上世纪的另一个巨大进展是所谓语义网络的表示方法[4]. 语义对象为一个节点，而节点之间的联系用边表示。这种方法既能用于[表示词汇语义](https://book.douban.com/annotation/77698180/)，又能表示句子语义，如下图所示。需要额外强调的是语义网络（semantic network）和后来在万维网基础上进一步发展的语义网概念（semantic web）完全完全不同。

<img height="250px"
     src="/assets/imgs/2019/0622-semantic-network.png"
     alt="Semantic Network"
/>

后来人们提出了框架[5]的概念，即人类认知可以用一个框架来解释，框架之中包含了各种成分，跟上面讨论 Davidsonian 的“事件”类似，框架是作为一个整体被人们认知并记住的。FrameNet 就是这样的一个框架资源。它定义了很多个框架，每个框架可能在句子中由一些具体动词、名词来触发，而这个框架作为认知概念也有很多预定义的槽，例如结婚事件就有起止时间、结婚双方、结婚地点等等。同时框架之间也可能有关系，比如上下位关系，或者因果关系等。

与基于框架的 FrameNet 不同，PropBank 资源库中针对对每个英语动词做了规定，每个动词可能有arg0、arg1、arg2等等参数，但它们尽量遵守了语义角色的约定。而VerbNet也是针对每个动词，但是每个动词的参数是通过语义角色的封闭集合来定义的，不像PropBank一样用0、1、2、3来表示。这几类资源库在NLP中非常有用，可以用 [Unified Verb Index](http://verbs.colorado.edu/verb-index/index.php) 这个网站统一查到每个动词在所有资源库中究竟怎么表示的。这里的框架与目前NLP中流行的“事件”[6]比较相似，但略有不同。目前的框架资源是基于词汇的定义，而事件可以针对领域单独定义。

从分类的角度上，框架之后人们又提出了脚本的概念。文本中常见的一些概念集合或序列形成了固定模式，这些模式就作为脚本来理解。例如餐厅就餐、公司上市等，都是包含了诸多事件的脚本。下图是一个大段文本中抽出部分脚本内容的例子。

<img height="250px"
     src="/assets/imgs/2019/0622-script.png"
     alt="Script"
/>

在框架、脚本等概念之上，本体(Ontology)是最为系统性和整体性的一种表示。本体论通常指对特定领域或微世界（microworld）分析产生的不同对象的集合。分类学（taxonomy）则将本体论中的元素以特定形式安排为树形结构，以阐述它们的类包含关系。如下图，生物领域的分类体系是展示本体论的最佳示例。

<img height="250px"
     src="/assets/imgs/2019/0622-biology-ontology.png"
     alt="Ontology and Taxonomy in Biology"
/>

说远了，回到语义表示上来，虽然说框架、脚本等资源在NLP中很重要，语义表示依然在近年有非常多的进展。

一种是English Resource Semantics。它是基于Minimal Recursion Semantics的一种实践，有flat、underspecified等优点（可参考[过去写的slides](https://speakerdeck.com/zxteloiv/semantic-parsing-methods?slide=36)）。具体来说由同步超边替代语法（SHRG，一种上下文无关语法）、并考虑了一些英语特性写的ERG（English Resource Grammar）表示出来的语法。近些年的研究中出现得不多了，可以参考其[官网](http://moin.delph-in.net/ErgSemantics)。

另一种必须提到的语义表示是 AMR。它基于PropBank对动词的定义，但加入了很多扩展，旨在构造一个与语言表达无关的语义形式，即相同的意思若用不同的方式说出来（名词化、主动、被动等）其语义表达不变。如下为AMR的一个表示例子，具体可以参考[AMR官网](https://amr.isi.edu/)。

<img height="250px"
     src="/assets/imgs/2019/0622-AMR.png"
     alt="AMR"
/>

最后，近年来斯坦福大学提出了一种统一句法表示（[Universal Dependencies](https://universaldependencies.org/)， UD），它的目标是针对一切语言做一个统一的句法表示，目前支持了数十种语言。虽然UD主要是表示句法的，但逐渐也融入了一些语义的特点。Reddy等人提出了使用这种通用句法表示去得到一个通用语义表示的方法[7]。

句法层面虽然各种各样，但人们希望努力得到一个统一的语义表示，这方面的努力是目前提出的一些新语义表示所共有的。最后强烈推荐这方面的一篇文章《The State of the Art in Semantic Representation》[8]，发表于ACL 2017，介绍了诸多语义表示的相似和不同。

# 5. 小结

本文介绍了N元关系的一些基本背景，以及最近的几种N元表示模型：

- 子属性模型
- RDF Reification模型
- 基于Neo-Davidsonian的模型
- FrameBase

最后还强行写了一些语义表示的相关工作，希望对大家有用。

# 参考文献 

[1] Grefenstette, E., 2013. Towards a Formal Distributional Semantics: Simulating Logical Calculi with Tensors. Second Joint Conference on Lexical and Computational Semantics (*SEM), Volume 1: Proceedings of the Main Conference and the Shared Task: Semantic Textual Similarity 1, 1–10.

[2] Martin, J.H. and Jurafsky, D., 2009. Speech and language processing: An introduction to natural language processing, computational linguistics, and speech recognition. Upper Saddle River: Pearson/Prentice Hall.

[3] Liang, P., Jordan, M.I., Klein, D., 2013. Learning dependency-based compositional semantics. Computational Linguistics 39, 389–446.

[4] Quillan, M.R., 1966. Semantic memory. BOLT BERANEK AND NEWMAN INC CAMBRIDGE MA.

[5] Fillmore, C.J., 1985. Frames and the semantics of understanding. Quaderni di semantica 6, 222–254.

[6] Doddington, G.R., Mitchell, A., Przybocki, M., Ramshaw, L., Strassel, S., Weischedel, R., 2004. The Automatic Content Extraction (ACE) Program – Tasks, Data, and Evaluation. Presented at the Proceedings of the Fourth International Conference on Language Resources and Evaluation (LREC’04).

[7] Reddy, S., Täckström, O., Petrov, S., Steedman, M., Lapata, M., 2017. Universal Semantic Parsing. Presented at the Proceedings of the 2017 Conference on Empirical Methods in Natural Language Processing, pp. 89–101. https://doi.org/10.18653/v1/D17-1009

[8] Abend, O., Rappoport, A., 2017. The State of the Art in Semantic Representation. Proceedings of the 55th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers) 1, 77–89. https://doi.org/10.18653/v1/P17-1008

