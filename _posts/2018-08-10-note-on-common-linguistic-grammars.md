---
layout: post
title:  "Relations among the Grammar Frequently Used in NLP"
date:   2018-08-10 22:30 +0800
tags: grammar NLP
categories: note
---

Grammar is a common and ambiguous term in NLP. I have done some study on wikipedia and some other linguistic textbooks for a clear understanding.

> This note serves as a memo and try to explain the relation between each of them in utterance, although a knowledge graph might be more appropriate and better represents what I want to convey.

## 1. Formal Language Aspect of Grammar

From the aspect of formal languages, we know there are mainly four types of grammar.

- Type-0: Unrestricted
- Type-1: Context-Sensitive Grammar
- Type-2: Context-Free Grammar
- Type-3: Regular Grammar

The larger the number, the stricter and less expressive the grammar. By _less expressive_ we means the less language could be generated from that grammar.

It is common to augment a CFG with some extensions, making it more powerful and expressive. Some examples are as follows,

- Generalzed CFG
- Linear Context-Free Rewriting Grammar, which as its name suggested is a subclass of GCFG
- Head Grammar is less powerful LGCFG
- Indexed Grammar is another kind of CFG
- Linear Indexed Grammar is a specialized Indexed Grammar

## 2. Linguistic View of Grammar

On the other linguistic side, people are also talking about grammars, which try to explain how human languages are generated and understood.
Though they could be classified as some of the four types above, it is usually not the key point to proclaim or prove that.
There are some philosophical debate and different approaches for grammars in natural language.

There are 2 main approaches to describe grammar or word relations in human language, namely the **consituency relation** and the **dependency relation**.
There are also other important approaches like cognitive grammar, functional grammar and stochastic grammar, which will not be covered in this post.

The **_Consituency Relation_** side mainly focus on syntactic structures like phrases, e.g. noun phrases, verb phrases, extracted from sentences.

- Phrase Structure Grammar
- General Phrase Structure Grammar
- Head-driven Phrase Structure Grammar(HPSG)
- Lexical Functional Grammar(LFG)
- Minimalist Programm, which favors _bare phrase structure_ and abandoned _X-bar theory_
- Nanosyntax
- Arcpair Grammar
- Categorial Grammar, and the famous Compositional Categorical Grammar(CCG)

Note the last two are not directly originated from Chomsky's theory.

And the _English Resources Grammar_(ERG) is a hand-crafted, linguistic-motivated HPSG for English only, and produces _English Resource Semantics_(ERS). But this lies on the semantic topic, which will not be covered by the post.

And here is the **_Dependency Relation_** side. Rather than composing words into phrases, this approach emphasize relations between any two words in a sentence.

- Recursive Categorical Syntax(algebraic syntax)
- Functional Generative Description
- Lexicase
- Link Grammar
- Meaning-text Theory
- Operator Grammar
- Word Grammar

There're common grammars not belonging to any of the two approaches. For example,

- Tree Adjoining Grammar(TAG)
- Lexicalized Tree Adjoining Grammar(LTAG), a TAG armed with lexical surface forms

Although it is possible to classify these grammar to Type-0/1/2/3, that's not critical questions thus in literature they are usually discussed in parallel to the grammar of formal language.

## 3. Equivalence between Grammars

Since there're so much grammars so far, it's natural to compare any two of them. For example, how powerful is some one compared to another, i.e. the equivalence between two grammars.

> Two grammar are weakly equivalent, iff they can generate the same language string.   
> Two grammar are strongly equivalent, iff they can generate not only the same language string but also the derived tree.

It has been proven that these grammar are weakly equivalent: _Head Grammar_, _Linear Indexed Grammar_, _Compositional Categorical Grammar_(CCG), and _Tree Adjoining Grammar_(TAG).
And _Lexicalized Tree Adjoining Grammar_(LTAG) is strongly equivalent to the _Head-driver Phrase Structure Grammar_(HPSG).

Then we can conclude the expressive power of them, from less expressive to stronger:

- Regular Grammar
- CFG
- GCFG
- LCFRS
- Head Grammar, Linear Indexed Grammar, CCG, TAG, LTAG, HPSG
- CSG
- Unrestricted


