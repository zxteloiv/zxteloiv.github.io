---
layout: post
title:  "Personal Reviews on Bayesian Networks"
date:   2015-08-18 22:37
tags: bayesian-network
math_support: katex
categories: main learning
---

Bayesian network is a basic but useful tool to many application problems. It was invented very early and is now one of the fundamentals in graphical models.   
Here I reviewed some of its key points which I summarized from mainly two books cited at the end of the post. 

### 1. Structure Modeling

When it comes to the probablistic learning problem, a joint distribution with many variables could be very difficult to deal with, because the possible ways those variables interact could be too many.

A joint distribution with 10 variables, for example, could be written as $$ P(X_1, X_2, X_3, ..., X_{10}) $$. If all these variables are binary, the gross parameters we need to specify the distribution would be $$2^n-1$$, which is already a tremendous number even if n is only 100 or so.

One way to solve the problem is to introduce some dependencies on these variables. If these variables are independent to each other, that is, in the most extreme case, we may have
$$ P(X_1, X_2, ..., X_{10}) = \prod_{i=1}^{10}P(X_i) $$.  
Using the result, only 10 parameters are necessary to compute a joint distribution, if the variables are still binary. So this really saves a lot.

In this way, we actually assume that these variables are all disjoint and independent to eath other, that they have the most simple structure. This structure could be drawn out graphically. We can therefore use other structures based on the concrete problem. These structures help us factorize the joint distribution into seperate factors like the example above.

Bayesian network or belief network is one way we can view the original model with its intrinsic structure, and factorize the distribution over some kind of graph.

### 2. Uncertain and Unreliable Evidence

This section is about how we can deal with variables we are uncertain about. These two type of evidence may not be something important in Bayesian Network but is still useful and interesting.

#### Uncertain evidence
Uncertain evidence is a variable that is not sure about whether it is in some state, which is also called a soft evidence, differing from the hard evidence we've known before which could have only one state at a specific time. For example the yellow color is a mixture of red and green, so color can be deemed as the soft evidence.

A binary variable "A", say, could have 70% chances to be 0 and 30% chances to be 1, because of the limits of our measure device or our carelessness when observing. Anyway, the binary variable itself may have a distribution now. What could we do if we are facing the uncertain evidence?

The quick answer is, take it as a normal variable and use bayesian rule. Let $$\tilde{A}$$ be the uncertain evidence and A is the real variable.
$$
P(B|\tilde{A}) = \sum_A P(B|A)P(A|\tilde{A})
$$
Because P(B|A) is already known at the inference time and $$P(A|\tilde{A})$$ is given above, we can compute the left side out by summing over A. This procedure is also known as Jeffrey's Rule.

#### Unreliable evidence
Unreliable evidence is a little different. This time it is we ourselves that distrust the evidence, whereas the evidence itself is not unsure. Let's take a door bell and an old man as an example. If the bell ever rang, there could be 70% probable for the old man to report he has heard of it. If it never rang, there could be 30% probable for him to report he has heard of it still.

And this time we could introduce a _virtual evidence term_ to show P(H\|A) = 0.7 for A = true, while P(H\|A) = 0.3 for A = false. Then use bayesian rule to sum over it again.

### 3. Independence for Bayesian Networks

When a distribution factorizes over a graph, we can read the independence between many variables from it.

A bayesian network is defined as 

$$
p(x_1, x_2, ..., x_n) = \prod_{i=1}^n p(x_i | pa(x_i)).
$$

where pa(x) is the parent nodes for x.

The graph for the distribution is actually a Directed Acyclic Graph (DAG).

For convenience, we only consider three different structures that are common in almost all graphs, and consider to maginalize or condition on some variables. Then we see how the independence may vary in different settings.

#### Scene 1
Graph nodes are formed like: $$ x \rightarrow z \rightarrow y $$. Then we have $$ p(x, y, z) = p(x)p(z|x)p(y|z) $$

Let's consider maginalization (over z) first.

$$
p(x, y) = \sum_z p(x, y, z) = \sum_z p(x)p(z|x)p(y|z) = p(x) \sum_z p(z|x)p(y|z) \neq p(x)p(y)
$$

So x and y are actually **dependent**. In fact we can see it directly from the graph, x _causes_ z and z then _causes_ y, so they may affect each other.

But when conditioning on z, things are different.

$$
p(x, y | z) = \frac {p(x, y, z)} {p(z)} = \frac {p(x)p(z|x)p(y|z)} {p(z)} = \frac {p(x,z)p(y|z)} {p(z)} = p(x|z)p(y|z)
$$

So x and y are really **independent** conditioning on z.

#### Scene 2
Here is another graph, which looks like: $$ x \leftarrow z \rightarrow y $$, which gives $$ p(x, y, z) = p(z)p(x|z)p(y|z) $$.

Similarly let's maginalize z first.

$$
p(x, y) = \sum_z p(x, y, z) = \sum_z p(z)p(x|z)p(y|z) \neq p(x)p(y)
$$

So x and y are *dependent*.

And let's conditioning on z, we have

$$
p(x, y | z) = \frac {p(x, y, z)} {p(z)} = \frac {p(z)p(x|z)p(y|z)} {p(z)} = p(x|z)p(y|z)
$$

So x and y are *independent* given z.

#### Scene 3
Finally the graph is like: $$ x \rightarrow z \leftarrow y $$, that gives $$ p(x, y, z) = p(x)p(y)p(z|x, y) $$.

When we are maginalizing z, 

$$
p(x, y) = \sum_z {p(x, y, z)} = \sum_z {p(x)p(y)p(z|x, y)} = p(x)p(y)
$$

It says that x and y are *independent*.

When we are conditioning on z,

$$
p(x, y | z) = \frac {p(x, y, z)} {p(z)} = \frac {p(x)p(y)p(z|x, y)} {p(z)} \neq p(x|z)p(y|z)
$$

So x and y are *dependent* if z is given.

#### Summarize

Let's put all the results above together. Here in Koller's book[1] it is much clearly and intuitively, so I adopt her words to summarize.

We call the third scene a **v-structure**, in which two arrows point towards an observed variable or an ancestor of an observed variable.   

And still, we define an **active trails**. A trail $$ X_1, X_2, ... X_n $$ is active given Z if the following two assert holds:

1. For any v-structure x-z-y, z or one of its descendents is in Z (observed)
2. For any other x, x is not in Z (not observed)

Then we could talk about the characteristics for active trails. An active trail can "transfer the influence or effect along its path". If a trail is not active, it will *block* the flow of influence, so the variables at the ends of the trail will be independent.

Refer to Barber's book[2] you will find another definition.

### 4. Graphical and Distributional Independence

Up to now I only understand that the independence in an distribution could be more than that of a graph. And also a distribution could have more graph compatable to it, since the influence is in bidirection while the network graph suggested the causality of only one variable on another. So this section remains under constructionˊ\_>ˋ


### Bibliography

1.Koller, D. & Friedman, N. Probabilistic Graphical Models: Principles and Techniques. (MIT Press, 2009).   
2.Barber, D. Bayesian Reasoning and Machine Learning. (Cambridge University Press, 2011).


