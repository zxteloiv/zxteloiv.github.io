---
layout: post
title:  "Word Representation Revisited"
date:   2015-11-09 20:00
tags: learning natural_language_processing word_vector word_representation word_embedding
categories: note learning
math_support: mathjax
---

This is a rearranged note for the first two lectures of the course [CS224d][CS224d] taught at Stanford this year.  
I would cover how we come to the very place of word embedding and what we've ever done along this route.  
Note that there's still some points not clear and I need to refer to some paper and revisit this post in several days.

## Word Representation from the Beginning

At first people did research at word representation in several methods, like taxonomy and synonym.

WordNet is an example of taxonomy. It represents the words in their hypernym relation, like a eagle "is a" bird.

Synonym is another set that put words together of similar meaning.

But these kinds of work have problems.

* New word is hard to be added into the current set.
* The arrangement is subjective.
* It needs much human efforts to build such a set or WordNet.
* It's hard to accurately assess the similarity between words in the resource.

## SVD methods

First people try to build a word-document matrix. Each position $$(i, j)$$ contains the number of times a word $$w_i$$ appears in a document $$d_j$$. Then the matrix has the shape of $$\vert V\vert \times \vert D\vert $$.  
This kind of matrix give rise to researches in topic models for a document, like LSA, PLSA, LDA and so on. In this post it will not be covered.

Another kind of matrix is word-word matrix. Each position $$(i, j)$$ contains the number of co-occurence time of word $$w_i$$ and $$w_j$$. In this way, the matrix has the shape of $$\vert V\vert \times \vert V\vert$$.

Using SVD we could decomposite the matrix into three matrices with the shapes $$\vert V\vert \times K, K\times K, K\times \vert V \vert$$ respectively.

Up to now, if we use the first matrix in the results of SVD, we could get some cool results. Each row of this matrix is a condensed word representation. Similar words are _clustered_ together though we didn't tell it any information. These are just statistical properties of the corpus.

But SVD methods do have other problems, too.

* The matrix size goes with our vocabulary size.
* It needs a lot of storage, and actually it's a very sparse matrix.
* The model is less robust because it may change a lot if our corpus or vocabulary varies.
* The matrix is high dimension and needs quadratic time to train (SVD).

We may do some hacks for some problems, too:

* For really high-frequency words like *the*, *a*, *is*, ...
    * those words capture much less semantics than the long-tail words
    * we may ignore them or set a upper bound (say 100) for the co-occurence count
* Words cooccurence may be weighed by their distance
* Use Pearson correlation instead of raw count **(??? but how)**
    * there are many words that do not occur together but it's meaningless to deal with them.
    * set to 0 if the correlation is negative

And the representations given by SVD are not ideal. We may want some think like $$w_{steal} - w_{stolen} \approx w_{take} - w_{taken}$$ to capture the syntactic or even semantic properties.

Then we move to iteration models, namely, each time only a small portion of data is processed.

## Continuous Bag of Words model.

Like the n-gram models, a sequence could be assigned with a probability. Again, they may need to compute on global data to get the probabilities.

In CBOW model, we use the surrounding to predict the central word. Take the following sentence as an example.

"The cat *jumps* over the puddle."

### model parameters

What we know: context words(like *cat*, *over*) in one-hot representation, and central word(*jumps*).

What we don't know: input matrix $$W^{(1)}\in \mathbb R^{n\times\vert V\vert}$$ and output matrix $$W^{(2)}\in \mathbb R^{\vert V\vert\times n}$$, where $$n$$ is just a constant we choose as the size of embedding space.

### loss function

The cross entropy is often used as the objective function in the setting when a probability is going to be learned from the real one. The cross entropy is defined as 

$$
H(\hat y, y) = -\sum_{i=1}^{\vert V\vert} y_i\log(\hat y_i)
$$

Here in fact $$y$$ is in one-hot representation so it's 0 everywhere except some place $$i$$.

$$
H(\hat y, y) = -y_i\log(\hat y_i) = -log(\hat y_i)
$$

### detail sequence

1. extract context embeddings $$u^{(i-C)}\dots u^{(i-1)}u^{(i+1)}\dots u^{(i+C)} $$, from $$W^{(1)}$$ matrix, for context words $$w^{(i-C)}\dots w^{(i-1)}w^{(i+1)}\dots w^{(i+C)}$$ respectively.
2. average the context vector to a single one $$h = \frac{\sum_j u^{j}}{2C}, j=(i-C)..(i+C)$$
3. get the output word $$z = W^{(2)}h$$
4. estimator $$\hat y=softmax(z)$$

Let $$v^{(i)}$$ be the output vector of the word $$w_i$$. (We are actually learning two vectors in $$W^{(1)},W^{(2)}$$ for a single word).  
Formally, the loss function will be like

$$
\begin{equation}
\begin{aligned}
\text{minimize J} &= -\log P(w^{(i)}\vert w^{(i-C)}\dots w^{(i-1)}w^{(i+1)}\dots w^{(i+C)}) \\
&= -\log P(v^{(i)} \vert h) \\
&= -\log \frac{\exp(v^{(i)T}h)}{\sum_j \exp(v^{(i)T}u^{(j)})} \\
&= -v^{(i)}h + \log \sum_{j=1}^{\vert V\vert}\exp(v^{(i)T}u^{(j)})
\end{aligned}
\end{equation}
$$

What might be ambiguious here is that the indice $$i$$ refers to many things:

* the order of the central word in a sentence
* the id of the i-th word in the corpus

Let's just assume they are the same. The corpus has only one sentence, and all words in the sentence is different.

## Skip-gram model

Skip-gram is just the opposite from the CBOW model. We predict the surroundings using the central word.

But I'm not sure how $$W^{(2)}h$$ could assign probabilities to various context word. **Revise here later**

## Word2Vec

Word2Vec uses a skip-gram model. The objective function is like:

$$
J(\theta) = \frac{1}{T}\sum_{t=1}^{T}\sum_{-c\le j \ge c, j\ne0} logP(w_{t+j}\vert w_t)
$$

The probability in the function above is computed using softmax:

$$
P(w_O\vert w_I) = \frac{\exp(v_{w_O}^Tu_{w_I})}{\sum_w\exp(v_w^Tu_{w_I})}
$$

Large vocabulary also makes the objective function hard to train. Two ways to solve it:

* Approximate
* Negative prediction: sample words not appear **negative sampling to revise later**

word2vec combines the two worlds:

| count-based | prediction based|
| ----------- | --------------- |
| LSA HAL COALS Hellinger-PCA | NNLM, HLBL, RNN, Skip-gram/CBOW |
| Fast training | Scale with corpus size |
| Efficient usage of statistics | inefficient usage of statistics |
| Primarily used of capture word similarity | generate improved performance on other tasks |
| Disproportionate importance to small counts | can capture complex patterns beyond word similarity |

**to be revised here**

Glove: combines both: fast training, scalable, small corpus is also good

$$
J = \frac{1}{2} \sum_{ij}f(P_{ij})(w_i \cdot \tilde w_j-\log P_{ij})^2
$$

**glove to be revised**

To get word analogies:

a:b :: c:d

$$
d = argmax_x \frac{(w_b-w_a+w_c)^Tw_x}{\parallel w_b-w_a+w_c \parallel}
$$

and Word embedding matrix can be "pre-trained" and used in later tasks.

## Bibliography

* Richard Socher,CS224d: Deep Learning for Natural Language Processing,2015,http://cs224d.stanford.edu/index.html
* Mikolov, Tomas et al. “Distributed Representations of Words and Phrases and Their Compositionality.” Advances in Neural Information Processing Systems 26. Ed. C. J. C. Burges et al. Curran Associates, Inc., 2013. 3111–3119. Neural Information Processing Systems. Web. 11 June 2015.
* Mikolov, Tomas et al. “Efficient Estimation of Word Representations in Vector Space.” arXiv preprint arXiv:1301.3781 (2013): n. pag. Google Scholar. Web. 11 June 2015.
* Pennington, Jeffrey, Richard Socher, and Christopher D. Manning. “Glove: Global Vectors for Word Representation.” Proceedings of the Empiricial Methods in Natural Language Processing (EMNLP 2014) 12 (2014): n. pag. Google Scholar. Web. 18 Apr. 2015.
* Huang, Eric H. et al. “Improving Word Representations via Global Context and Multiple Word Prototypes.” Proceedings of the 50th Annual Meeting of the Association for Computational Linguistics: Long Papers-Volume 1. Association for Computational Linguistics, 2012. 873–882. Google Scholar. Web. 7 Nov. 2015.

[CS224d]: http://cs224d.stanford.edu/syllabus.html
