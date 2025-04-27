---
layout: post
title:  "Reinforcement Learning Memo"
date:   2016-01-26 23:55
tags: machine_learning
categories: main learning
math_support: mathjax
---

This blog is a memo for reinforcement learning, inspired by an introductory note. I would suggest people read the [original post](http://www.nervanasys.com/demystifying-deep-reinforcement-learning/). This memo will give an outline for its thoughts and logic. This post lacks technical details. Referring to the paper listed at the end may be useful.

### Motivation

As the work by DeepMind stated, they've use reinforcement learning to play a game. If we want to teach machine to play a game, we may take it as a classification task. Given pictures of a screen, the model output an action. But this is not how we humans learn.

When we're playing a game, we choose a lot of actions to get a final reward. How a reward is related to each previous actions is the problem we are facing. That's known as **credit assignment problem**.

Another question is how much will an agent strive for the reward. Will it just try once and give up with a reward of very low points? This is **explore-exploit dilemma**.

### Formalization

Often we use **Markov Process** to model a game. The **agent** in specific **state** of the game **environment** must choose **actions** to get the **reward**, in special **policy**. The states in all therefore forms a Markov Process.

$$
s_0,a_0,r_1,s_1,a_1,r_2,...,r_n,s_n
$$

Total reward will be $$R=\sum_ir_i$$.

At time $$t$$, the total reward from now on will be $$R_t=\sum_{i=t}^nr_i$$.

Because the more we move towards into the future, the more likely the reward will be converge. The total reward will be **discounted**.

$$
R_t=\sum_{i=t}^n\gamma^{i-t}r_i=r_t+\gamma r_{t+1}+\gamma^2r_{t+2}+...+\gamma^{n-t}r_n\\
s.t. 0\le\gamma\le1\\
\therefore R_t=r_t+\gamma R_{t+1}
$$

A good policy would be to maximize the future reward.

### Q-Learning

Let Q function be $$Q(s_t,a_t)=\max R_{t+1}$$, which is the future reward we perform action *a* in state *s* and continue optimally from then on.

At time *t*, also define the chosen action as $$\pi(s_t)=\arg\max_aQ(s_t,a_t)$$.

As stated above, we compute Q function *iteratively* using the maximum future reward.

$$Q(s,a)=Q(s,a)+\alpha(r+\gamma\max_{a'}Q(s',a')-Q(s,a))$$

parameter alpha is something like the learning rate.

### Deep Q-Learning

But if the number of states is too much, we may turn to DNN for a better generalization.

![](http://www.nervanasys.com/wp-content/uploads/2015/12/Screen-Shot-2015-12-21-at-11.27.12-AM.png)

Like the structure above, we may use the right model since it computes the future reward for all action at once.

**Experience Replay**

Experience replay is a trick. We store all experience in the special memory, and use random minibatches from it to training. Maybe less likely will we go into local minimum.

**Exploration-Exploitation**

Use another $$\epsilon$$-greedy exploration technique. Use a probability to move randomly, which will decrease from 1 to 0.

### Bibliography

1. Blog post: [Demystifying Deep Reinforcement Learning](http://www.nervanasys.com/demystifying-deep-reinforcement-learning/) by nervana.
2. Reinforcement Learning [course by David Silver](http://www0.cs.ucl.ac.uk/staff/D.Silver/web/Teaching.html).
3. Reinforcement Learning [course from UC Berkeley](http://rll.berkeley.edu/deeprlcourse/).
4. Q-Learning detailed blog http://artint.info/html/ArtInt_265.html
5. Mnih, Volodymyr, et al. "Playing atari with deep reinforcement learning." *arXiv preprint arXiv:1312.5602* (2013). http://arxiv.org/abs/1312.5602
