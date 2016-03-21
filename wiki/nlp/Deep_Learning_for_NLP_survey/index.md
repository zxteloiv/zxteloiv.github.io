---
layout: page
title: Deep Learning for NLP survey
math_support: mathjax
---


http://nlp.stanford.edu/courses/NAACL2013/

# Deep Learning for Natural Language Processing (without Magic)

A tutorial given at [NAACL HLT    2013](http://naacl2013.naacl.org/). Based on an earlier tutorial given at     [ACL 2012](http://www.acl2012.org) by Richard Socher, Yoshua Bengio, and    Christopher Manning.

By [Richard Socher](http://www.socher.org/) and   [Christopher Manning](http://nlp.stanford.edu/%7Emanning/)

## Slides

[NAACL2013-Socher-Manning-DeepLearning.pdf](http://nlp.stanford.edu/courses/NAACL2013/NAACL2013-Socher-Manning-DeepLearning.pdf)	   (24MB) - 205 slides.

## Videos

[Part 1](http://techtalks.tv/talks/deep-learning-for-nlp-without-magic-part-1/58414/)

[Part 2](http://techtalks.tv/talks/deep-learning-for-nlp-without-magic-part-2/58415/)

*Sorry, Flash videos only.* ☹

## Abstract

Machine learning is everywhere in today's NLP, but by and large  machine learning amounts to numerical optimization of weights for  human designed representations and features. The goal of deep  learning is to explore how computers can take advantage of data to  develop features and representations appropriate for complex  interpretation tasks. This tutorial aims to cover the basic  motivation, ideas, models and learning algorithms in deep learning  for natural language processing. Recently, these methods have been  shown to perform very well on various NLP tasks such as language  modeling, POS tagging, named entity recognition, sentiment analysis  and paraphrase detection, among others. The most attractive quality  of these techniques is that they can perform well without any  external hand-designed resources or time-intensive feature  engineering. Despite these advantages, many researchers in NLP are  not familiar with these methods. Our focus is on insight and  understanding, using graphical illustrations and simple, intuitive  derivations. The goal of the tutorial is to make the inner workings  of these techniques transparent, intuitive and their results  interpretable, rather than black boxes labeled "magic here". The  first part of the tutorial presents the basics of neural networks,  neural word vectors, several simple models based on local windows and  the math and algorithms of training via backpropagation. In this  section applications include language modeling and POS tagging. In  the second section we present recursive neural networks which can  learn structured tree outputs as well as vector representations for  phrases and sentences. We cover both equations as well as  applications. We show how training can be achieved by a modified  version of the backpropagation algorithm introduced before. These  modifications allow the algorithm to work on tree  structures. Applications include sentiment analysis and paraphrase  detection. We also draw connections to recent work in semantic  compositionality in vector spaces. The principle goal, again, is to  make these methods appear intuitive and interpretable rather than  mathematically confusing. By this point in the tutorial, the audience  members should have a clear understanding of how to build a deep  learning system for word-, sentence- and document-level tasks. The  last part of the tutorial gives a general overview of the different  applications of deep learning in NLP, including bag of words  models. We will provide a discussion of NLP-oriented issues in  modeling, interpretation, representational power, and optimization.

## Outline

1. The Basics
   1. Motivations
   2. From logistic regression to neural networks
   3. Word representations
   4. Unsupervised word vector learning
   5. Backpropagation Training
   6. Learning word-level classifiers: POS and NER
   7. Sharing statistical strength
2. Recursive Neural Networks
   1. Motivation
   2. Recursive Neural Networks for Parsing
   3. Optimization and Backpropagation Through Structure
   4. Compositional Vector Grammars: Parsing
   5. Recursive Autoencoders: Paraphrase Detection
   6. Matrix-Vector RNNs: Relation classification
   7. Recursive Neural Tensor Networks: Sentiment Analysis
3. Applications, Discussion, and Resources
   1. Assorted Speech and NLP applications
   2. Deep Learning: General Strategy and Tricks
   3. Resources (readings, code, …)
   4. Discussion

## References

[All	   references we referred to in one pdf file](http://nlp.stanford.edu/%7Esocherr/DeepLearning-ACL2012-tutorial.pdf)

## Further Information

- A very useful assignment for getting started with deep learning    in NLP is to implement a simple window-based NER tagger in this    exercise we designed for the [Stanford NLP class cs224N](http://cs224n.stanford.edu/).     The zip file    includes starter code in Java and the pdf walks through all the    steps:[http://nlp.stanford.edu/~socherr/pa4_ner.pdf](http://nlp.stanford.edu/%7Esocherr/pa4_ner.pdf)[http://nlp.stanford.edu/~socherr/pa4-ner.zip](http://nlp.stanford.edu/%7Esocherr/pa4-ner.zip)
- The implementation assignment for a sparse    autoencoder can be found here: [exercise description pdf](http://nlp.stanford.edu/%7Esocherr/sparseAutoencoder_2011new.pdf) and [matlab starter code](http://nlp.stanford.edu/%7Esocherr/sparseae_exercise.zip) (11MB)
- [The    most extensive and thorough tutorial](http://deeplearning.net/tutorial/) for deep learning in     general is available at the     [deeplearning.net](http://deeplearning.net/) site (using Theano, a Python library, from Yoshua    Bengio's group, which has its own [tutorial](http://deeplearning.net/software/theano/tutorial/))
- Another     [introductory tutorial](http://deeplearning.stanford.edu/wiki/index.php/UFLDL_Tutorial) from Stanford, which also    includes     [pointers to the implementation    assignment for a sparse autoencoder](http://deeplearning.stanford.edu/wiki/index.php/Exercise:Sparse_Autoencoder)
- A hands-on tutorial for denoising autoencoders can be found    at [http://deeplearning.net/tutorial/dA.html#daa](http://deeplearning.net/tutorial/dA.html#daa)
- Charles Elkan wrote a great, detailed derivation of recursive    neural networks: [http://cseweb.ucsd.edu/~elkan/250B/learningmeaning.pdf](http://cseweb.ucsd.edu/%7Eelkan/250B/learningmeaning.pdf)
- You can study clean recursive neural network code with    backpropagation through structure on this    page: [Parsing    Natural Scenes And Natural Language With Recursive Neural    Networks](http://www.socher.org/index.php/Main/ParsingNaturalScenesAndNaturalLanguageWithRecursiveNeuralNetworks)
- Learn about    [Deep      Learning for Natural Language Processing](http://nlp.stanford.edu/projects/DeepLearningInNaturalLanguageProcessing.shtml) research from the     [Stanford NLP Group](http://nlp.stanford.edu)




