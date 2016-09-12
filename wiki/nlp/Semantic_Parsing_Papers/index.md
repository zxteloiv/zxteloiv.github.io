---
layout: page
title: Semantic Parsing Papers
math_support: mathjax
---


## Zelle, Mooney, 1996

Learn to Parse Database Queries.

### tags

variable-free logic, shift-reduce parsing, inductive logic programming

### contents

Use database schema:

- basic objects: cityid, stateid, ...
- basic predicates: capital(), city(), major, ...
- meta predicate (high-order predicates): answer / largest / lowest / count ...

## Zelle, Mooney, 1993

Learning Semantic Grammars with Constructive Inductive Logic Programming

semantic grammar acquisition viewed as the learning of search control heuristics in a logic program

### tags

shift-reduce, 

### contents

![QQ20160908-0@2x.png](resources/6484A607496A2E429DEA709BFA3C1A4D.png)

#### flaw of connectionist models

1. output structure is flat, thus it's unclear how embedded propositions can be handled
2. give only a single output structure even if the sentence is truly ambiguous

#### CHILL (constructive heuristics induction for language learning)

1. introduce an overly-general parser using training data (may give spurious analyses)
2. specialize the parser by search control heuristics

CHILL Induction

![QQ20160908-2@2x.png](resources/0BA07F0491682954232BF3B6172AE5F9.png)

For each pair clause in sample:

- **Find_Generalization**: takes the pair, construct a new clause that subsumes them while not covering any negative example
- **Reduce_Definition**: uses new clause to prove positive examples. New clause is more prefered and any clause not used is deleted.

Until the generalized definition will not change.

## Zettlemoyer, Collins, 2005

Learning to Map Sentences to Logical Form: Structured Classification with Probabilistic Categorial Grammars

### tags

probabilistic CCG, log-linear model, sentence-LF pair,

(CKY, high-order logic)

### background

#### lambda calculus

lambda expressions have:

- constants: entity / number / function
- logical connectors
  - conjunction $\wedge$
  - disjunction $\vee$
  - negation $\neg$
  - implication $\to$
- quantification: $\forall, \exists$
- lambda-expressions $\lambda x.borders(x, texas)$
- additional quantifiers $count, \arg\max, \arg\min, \iota$
  - a definite operator would return the unique item for which state(x) is true, if a unique item exists
  

#### CCG

Combinatory Category Grammars (Steedman, 1996, 2000)

- lexicon: word to category
- category: (S\NP)/NP
- composition rules:
  - functional application (left / right)
  - type raising


### parsing

#### parsing uses PCCG

ambiguity comes from:

- lexical item with multiple entry in lexicon
- spurious ambiguity: same semantic but different derivation

log-linear model: (L=logical form, T=derivation sequence, S=sentence)

$$
P(L, T \mid S; \bar\theta) = \frac
  {\exp(\bar f(L,T,S)\cdot\bar\theta)}
  {\sum_{(L,T)}\exp(\bar f(L,T,S)\cdot\bar\theta)}
$$

and parsing involves PCCG inference:

$$
L = \arg\max_L P(L\mid S;\bar\theta) = \arg\max_L\sum_TP(L,T\mid S;\bar\theta)
$$

use dynamic programming(beam-search actually): features are only **local** lexical feature (number of occurence of a lexical entry in T)

#### estimation of theta

T is latent. maximize the log-likelihood.

$$
\begin{align}
O(\bar\theta)
  &= \sum_i\log P(L_i\mid S_i; \bar\theta) \\
  &= \sum_I\log (\sum_T P(L_i, T\mid S_i; \bar\theta))
\end{align}
$$

differentiate it w.r.t theta and then use dynamic programming (variant of inside-outside algorithm) 

### induce the lexicon

#### GENLEX

GENLEX generates lots of lexical entries of which some may be pruned later.

$$
GENLEX(S,L) = \{x := y \mid x \in W(S), y\in C(L)\}
$$

- W(S) is all subsequence of S
- C(L) maps L to a set of categories using rules which will produce categories when triggered by a constituent of logical forms.

![QQ20160910-0@2x.png](resources/B58945F8CA44CE38DC6F210EAFCE921B.png)

all possible lexicals:

$$
\Lambda^* = \Lambda_0 \cup \bigcup_{i=1}^n GENLEX(S_i, L_i)
$$

Weights are set to 0.1 for entries in Lambda_0 and 0.01 for others.

#### learning

basically two steps:

![QQ20160910-1@2x.png](resources/CEC6AD08D0189347857AD046E2590816.png)

- step1 produce a lexicon Lambda\_t compacter than Lambda^star
- step2 keeps the log-likelihood be optimized for Lambda_t

PARSE function will parse the tree and gives the highest probabilities.

### problems

GENLEX is controlled by rules, and will be insufficient if the rules don't cover all the sentence - logical forms. (lexicon and composition can both limited recall) e.g.

> Through which states does the Mississippi run.

GENLEX doesn't trigger a category suitable for the _through_-adjunct be placed ahead.

## Wong, Mooney, 2006

Learning for Semantic Parsing with Statistical Machine Translation

### tags

NAACL-2006, SCFG, WASP, MT technique for alignments,

robustness to variations in:

- task complexity
- word order

### model

#### synchronous CFG:

![QQ20160911-0@2x.png](resources/D594E4BE52165BC53A57FD20B84228D5.png)

![QQ20160911-1@2x.png](resources/2687A6CB624705432194DDD938CAE92B.png)

start from a pair of start symbol (s, s), get a pair as (e, f).

In each grammar rule $$X\to\langle\alpha,\beta\rangle$$, alpha is called a pattern, beta is called a template (Kate et al. 2005)

WASP uses SCFG to model alignments. 

#### parsing

parsing uses a probabilistic model to choose between different derivations

$$
f^* = m(\arg\max_{\mathrm{d}\in D(G\mid e)}\Pr(\mathrm{d}\mid e; \lambda))
$$

### learning

learning needs to 

- induce a set of rule (lexicon)
- a probabilistic model for derivations

#### alignment model

directly using MT techniques does not use MRL grammar, thus **allocation probability mass to MR translations that are not syntactically well-formed**.

alignments of NL words and MR token:

- not all MR token carry specific meanings (e.g. braces)
- polysemy in MR tokens (in CLang, pt stands for point and position)

conceptual alignment between NL words and MR tokens:

|        | 0 token | 1 token | multiple tokens |
| ---    | ---     | ---     | ----            |
| 0 word | -       |  braces | -               |
| 1 word | -       | polysemy in tokens or synonym in words | - |
| multiple words | - | - | - |

To simply avoid these problems, present an MR in a production sequence, which correspond to both

- Top-down
- left-most

derivations .

**MRL grammar** is (or can be made) unambiguous, thus the sequence is unique for a parse.

Grammar + Parse = >  sequence of productions.

Use GIZA++ (Och and Ney, 2003) to obtain the alignments of words and production rules, under the ASSUMPTION: word to production is n-to-1

![QQ20160911-2@2x.png](resources/B7C2365868BAD9190158A1213EF502F8.png)

#### extraction rules from alignments

bottom-up

RHS only contains terminals:

alignment: (our, Team -> our), extract: Team -> (our, our)

RHS also contains non-terminals:

extract: above

Problem: when rules cannot be extracted for some production

use a greedy remove to do alignment fixing

## Wong, Mooney, 2007

Learning Synchronous Grammars for Semantic Parsing with Lambda Calculus

### tags

ACL2007, Synchronous Grammar, lambda-WASP

mainly an improvement over WASP

### modeling and learning

problems: SCFG cannot handle logic variables, ZC05 is robust but still needs hand-written rules.

each rule(production) in SCFG is extended with:

$$ A\to\langle\alpha,\lambda x_1 \cdots\lambda x_k.\beta\rangle$$

under the ASSUMPTION: word to production is n-to-1

### optimization

#### alignment fixing

again the alignment can cause problem that rules cannot be extracted for some predicate.  -> parse and NL are not isomorphic.

To adjust the LF to improve isomorphism, using a graph algorithm.

![QQ20160912-0@2x.png](resources/89FA19D4F19264097249094DB3DD749A.png)

#### MRL language model

- type checking to omit unreal LF parse
- add new features to trival LF, the the number of times a given rule is used to expand a non-terminal in a given parent rule

![QQ20160912-1@2x.png](resources/CFFE85655233B886A5FDFCC3FDE4B82A.png)
































