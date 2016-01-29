---
layout: post
title:  "ClueWeb09 数据集把玩日记（下）"
date:   2016-01-29 14:00
tags: learning ClueWeb09 dataset 
categories: chn learning
math_support: mathjax
---

这篇主要包括怎么抽取一个网页的内容，和怎么发现瓶颈并优化性能的。代码都放在 [GitHub](https://github.com/zxteloiv/ClueWeb09-scripts) 上面。有的正则表达式和代码逻辑直接看代码可能更清楚一点。前一篇在此：[ClueWeb09 数据集把玩日记（上）](http://libzx.so/chn/learning/2016/01/28/play-with-clueweb09-and-facc1-1.html)

### 网页抽取

通过前面拿到了 ClueWeb09 的数据集，可以挨个读出 HTML 数据进行处理了。

这次的目标是把 FACC1 中标注的实体取出来，找到对应 HTML 数据中所在句子。由于直接用给定的字节位置难以确定前后的句子边界，同时一个文档中的同一个实体基本上多次出现时都是一样的，因此决定先提取句子，再找到标注实体出现在哪些句子当中。

首先涉及到提取 HTML 中句子的工作。Python 下直接用 bs4.BeautifulSoup（下面使用 bs 表示）就可以处理。然而踩到了不少坑：

- bs.get_text() 提取出的内容可能是多个句子的段落，也可能是一个句子的多个碎片。
- bs.get_text() 默认把子标签的文字提取出来不加分隔符。
- 迭代获取 bs.descendants 会得到子标签列表，把所有子标签都作为列表元素。从而如果叶子上有一个句子，则这个句子在多个列表元素上调用 get_text() 时都会被重复提取，甚至可能一个句子的多个碎片重复出现，无法排重。

唯一的办法就是直接使用 bs.get_text，不进行节点遍历。经过多次测试，总结出一些原则：

- 如果 html 内容以  HTTP/1.1 200 OK 等头信息开头，全部去掉。
- 去掉 html 中 <script [type="text/javascript"]> ... </script> 和 <style> ... </style> 标签的内容
- 去掉所有的注释 <!-- ... -->
- 去掉所有长度为0的句子
- 把 u'\xa0' 换成普通空格，这是在 bs 解析时把 html 中的 `& nbsp;` 等 HTML entities 进行替换所转换得到的空格，在 ASCII Table 中有规定
- 因为 HTML 不会把 <br> 以外的换行解释为换行，故把连续出现的多个 \n, \t, \r, 空格等等替换成单个空格。

在经过这些处理之后，把提取出的段落用 \n 分割，可以用 python requests wrapper 交给 CoreNLP Server 去处理。这部分使用了大量正则表达式进行处理，如果需要可以参考一下 re 模块的说明，例如 .*? 加了问号可以表示non-greedy 模式的 match、(?is) 开头可以空匹配但是指定整个表达式的匹配约定等等。

~~~python
def preprocess(html_text):
    # discard html headers
    html_text = re.sub('^HTTP[^<]*<', '<', html_text, flags=re.DOTALL)

    # discard possible comments in raw html
    html_text = re.sub(r'<!--(.*?)-->', ' ', html_text, flags=re.DOTALL)

    # Since new line feeds in HTML have no use, 
    # remove all new lines in the original text before we do,
    # because we will use the line feed as separator for differenct sentences.
    html_text = re.sub(r'[\r\n]+', ' ', html_text)
    html_text = re.sub(r'  +', ' ', html_text)

    return html_text

def get_sentences_from_html(html, nlp=None):
    from bs4 import BeautifulSoup
    soup = BeautifulSoup(html, 'lxml')

    # discard script and css style tag
    for tag in soup(['script', 'style']):
        tag.extract()

    # extract text from all tags.
    text = soup.get_text(separator=u'\n')
    paras = text.splitlines()

    # substitute all non-breaking space to normal space
    paras = (re.sub(u'\xa0', ' ', p) for p in paras)
    # discard comment
    paras = (p for p in paras if not (re.match(r'^[\r\n\t ]*<!--', p)))
    paras = (re.sub(r'<!--[^>]+-->', u' ', p) for p in paras)
    # substitute continuous blanks
    paras = (re.sub(r'\t+', u'\t', p) for p in paras)
    paras = (re.sub(r'[\r\n]+', u'\n', p) for p in paras)
    paras = (re.sub(r'  +', u' ', p) for p in paras)
    # discard empty string
    paras = (p.strip() for p in paras if len(p.strip()) > 0)

    # CoreNLP wrapper 使用 requests 库，传输 POST 数据时默认用 ASCII decode，需要显式使用 utf-8
    sentences = list(s for s in nlp_analyze(u"\n".join(p for p in paras).encode('utf-8'), nlp))

    return sentences
~~~

最后直接输出就好了。

### 性能调优

这里才是感觉最想分享的经验。前一篇说过了这个数据集非常大。于是花了一点时间用土办法做了并行化。跑起来定时用 wc -l * 看了一眼输出结果，计算了一下平均每秒才 44 条。这太慢了！

感慨了一番，工程师必须具备的能力除了实现功能之外，必须还有写测试、做性能 profiling。

于是研究了一下。Python 标准库里带了几个 [profiling 工具](https://docs.python.org/2/library/profile.html)，其中推荐使用 cProfile。

用起来非常简单：

~~~shell
$ python -m cProfile facc1.py FACC1/profile.ids.tmp /media/ClueWeb09/ClueWeb09_English_1/en0000/00.warc.gz 1> dump/all.out 2>dump/all.err
~~~

它把内容输出到 stdout，因此可能会和程序本身输出相混。我使用 -o profile.out 参数时输出的为乱码没有继续尝试。

~~~ 
         53951591 function calls (53948462 primitive calls) in 16.211 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.000    0.000 <string>:1(<module>)
        1    0.000    0.000    0.000    0.000 <string>:1(ArgInfo)
        1    0.000    0.000    0.000    0.000 <string>:1(ArgSpec)
        1    0.000    0.000    0.000    0.000 <string>:1(Arguments)
        1    0.000    0.000    0.000    0.000 <string>:1(Attribute)
        
                           ............
                           
       92    0.000    0.000    0.015    0.000 _lxml.py:53(parser_for)
       59    0.000    0.000    0.000    0.000 _lxml.py:62(__init__)
    97308    0.014    0.000    0.014    0.000 _lxml.py:72(_getNsTag)
      151    0.000    0.000   14.140    0.094 _lxml.py:80(prepare_markup)
        1    0.020    0.020    0.034    0.034 lxml.etree.pyx:1(PyMODINIT_FUNC PyInit_etree(void))
    37363    0.002    0.000    0.002    0.000 lxml.etree.pyx:123(__len__)
    
                           ............    
~~~

所有的子调用都会打出来，但对 lxml 这种 C 库只会跟踪到 wrapper 部分。各列分别含义是：总调用次数、不包括调用子函数的总时间、单个调用耗时、包括子调用的总时间、单个调用耗时、所属函数。

在 [stackoverflow](http://stackoverflow.com/a/11822995/2025428) 的一个问题下有人安利自己开发的可以把 cProfile 输出图形化的工具，需要安装 graphviz，但使用后可以看到一个 png 图片，方便直观可以尝试：

~~~shell
$ sudo apt-get install graphviz # ubuntu 下安装 graphviz 相关的包
$ pycallgraph graphviz -- ./mypythonscript.py
~~~

这里可以分析得到，bs4 中使用 lxml 是最耗时（60%）的部分（对 CoreNLP 的网络请求都没有那么多），因此这里是改进以后收获最大的部分。

重新回头看我们的程序，抽取网页并没有对 HTML 的节点进行操作，因此 parse 是没有什么作用的。考虑人工替换掉。参考之前的整理经验，实际上大部分句子并不是 HTML 中的换行得到的，而是交给了 CoreNLP 做解析。因此实际上是不是直接把 HTML 标签去掉就可以了呢？

搜索 Python 的 NLP 工具包实现 NLTK，[曾经有过](https://github.com/nltk/nltk/commit/39a303e5ddc4cdb1a0b00a3be426239b1c24c8bb) clean_html 的抽取函数，但后来也转向 bs4 实现了。

~~~ diff
 def clean_html(html):
-    """
-    Remove HTML markup from the given string.
-
-    :param html: the HTML string to be cleaned
-    :type html: str
-    :rtype: str
-    """
-
-    # First we remove inline JavaScript/CSS:
-    cleaned = re.sub(r"(?is)<(script|style).*?>.*?(</\1>)", "", html.strip())
-    # Then we remove html comments. This has to be done before removing regular
-    # tags since comments can contain '>' characters.
-    cleaned = re.sub(r"(?s)<!--(.*?)-->[\n]?", "", cleaned)
-    # Next we can remove the remaining tags:
-    cleaned = re.sub(r"(?s)<.*?>", " ", cleaned)
-    # Finally, we deal with whitespace
-    cleaned = re.sub(r"&nbsp;", " ", cleaned)
-    cleaned = re.sub(r"  ", " ", cleaned)
-    cleaned = re.sub(r"  ", " ", cleaned)
-    return cleaned.strip()
+    raise NotImplementedError ("To remove HTML markup, use BeautifulSoup's get_text() function")
 
 def clean_url(url):
-    html = compat.urlopen(url).read()
-    return clean_html(html)
+    raise NotImplementedError ("To remove HTML markup, use BeautifulSoup's get_text() function")
 
~~~

发现它使用的正则和我们之前的操作也差不多。于是新实现了一个 [get_sentences_from_html_v2() 函数](https://github.com/zxteloiv/ClueWeb09-scripts/commit/97cb3b1107aec9bf3ba36d19e719d88ef3e2a60f) 在之前 dump 出来的 html 文件样本上实验了一番，肉眼观察输出的句子效果很好，与 bs4 的功能差不多！

这下跑了一番，在一个 FACC1 文件的 500 个抽样上跑，原本需要 21 秒的程序，直接降到了 7.5 秒！巨大的提升，并且与 profile 的结论相近。

但是问题还没完，为什么 NLTK 会选择转向 bs4 呢？自然，HTML 本身并不是一个可以用正则语法囊括的，用正则表达式解析的做法本身就是走偏了，只能在很小的场景下使用。对应的 badcase 马上就被发现了：

~~~ html
<div onclick="if (a > 100) foo();">
.... blablabla ...  
</div>
~~~

面对这样的标签，在 onclick 事件代码中的 > 符号就引起了歧义，从而 100) foo();"> 这部分也被当成了句子。

解决办法就是用科学的方法解析 HTML tag，好在这里只需处理 tag，那就要想到自动机了。

首先花一天写了一个正常的自动机，支持状态之间的接受输入字符的转换、无输入字符的转换、没有合适输入时的默认转换。详细代码见 [naive_fsa.py](https://github.com/zxteloiv/ClueWeb09-scripts/blob/master/naive_fsa.py)，此处不再叙述。同时需要写一个 HTML 的 tag 的自动机转换规则，主要有两种：

~~~ html
<tagname attr="attrvalue"></tagname>
<tagname attr='attrvalue'></tagname>
~~~

因此写下 HTML Tag 自动机所需要的规则数据结构，详细可以看 [clean_html.py](https://github.com/zxteloiv/ClueWeb09-scripts/blob/master/clean_html.py)：

~~~ python
HTML_TAG_STATES = {

        "start": {
            "trans": [ ('<', 'tag_name') ]
            },

        "tag_name": {
            "trans": [ (' ', 'waiting_for_attr'), ('>', 'end') ],
            "default": "tag_name"
            },

        "waiting_for_attr": {
            "trans": [('>', 'end')],
            "default": "attr_name"
            },

        "attr_name": {
            "trans": [('=', 'attr_value_start'), ('>', 'end')],
            "default": "attr_name"
            },

        "attr_value_start": {
            "trans": [('"', 'attr_value_double_quote'), ('>', 'end')],
            "default": "attr_value_start"
            },

        "attr_value_double_quote": {
            "trans": [('"', 'end_of_attr_value')],
            "default": "attr_value_double_quote"
            },

        "attr_value_single_quote": {
            "trans": [("'", 'end_of_attr_value')],
            "default": "attr_value_single_quote"
            },

        "end_of_attr_value": { 
            "direct": "waiting_for_attr"
            },

        "end": {}

        }
~~~

至此，做完了 clean_html.py 以后整个解析程序完成。最后发现样本集上耗时 9.1 秒，相比直接正则多花了 2 秒左右，符合常理。

同时，由于我们已经摆脱了 bs4 和 lxml 的依赖，已经完全转到  pure python 上面来，可以直接改用 pypy 执行进行优化，最终使用 pypy 跑，耗时为 8.4 秒左右。

### 后续优化

实际上即使到最后每秒也只有 300 个左右的结果输出，还是太慢（全部处理需要133天且保证电脑不意外关机）.... 又要想别的办法了。

- 自动机的实现中，每次后移一位用暴力搜索，能否参考 KMP 的思想移动迅速一点？
- 健壮性：脚本多进程执行时依然出现了子进程挂掉的情况，在长时间、多进程的环境下这种情况是常态，必须考虑重新设计脚本接口，支持记录当前处理状态，并能死掉后重启。
- 日志的记录需要完备，避免过分依赖 stdout，同时也就可以方便实现监控了。这块功能可以形成一个框架性质的代码，方便将来的所有脚本进行重用。
- 实验 html 提取规则时很多拍脑袋的工作，能否更高效地优化规则提出的流程，是否有简单工具？
