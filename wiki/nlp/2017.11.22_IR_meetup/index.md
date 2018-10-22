---
layout: page
title: 2017.11.22 IR meetup
math_support: mathjax
---


# Reinforcement LtR

题 目 (TITLE) : 强化排序学习
讲 座 人 (SPEAKER)：徐君  中国科学院计算技术研究所研究员
主 持 人 (CHAIR)：刘康  副研究员
时 间 (TIME)：2017年12月22日 下午2:30-3:30
地 点 (VENUE)：智能化大厦三层第三会议室
报告摘要（ABSTRACT）：
排序学习研究在近十年来取得了长足的进展并在商业搜索系统中得到了广泛的应用，现有的排序学习模型大多基于相关性独立性假设，对待排序文档独立打分并排序。上述假设简化了排序流程和学习的难度，同时也限制了排序学习在更复杂排序场景和更多排序任务上的应用。本报告将介绍一种基于马尔科夫决策过程的强化排序学习框架，其将文档排序任务形式化为序列决策过程，每一次的决策依据当前状态进行，为当前位置选择一个文档，逐步构建文档序列。强化排序学习框架可以自然地建模包括多样化、交互式在内的多种排序学习任务，其已经被成功应用于相关性排序和多样化排序任务中并取得了显著的性能提升。

Independent Relevance Assumption: utility of a doc is independent of other docs
Beyond independent relevance: more ranking criteria (diversity) & complex acc env (interactive)
=> Ranking as Sequential Decision Making

![IMAGE](resources/31F5F013171B2D36169BBAD3FBDD1E3B.jpg =1920x1440)

# Q.A.

random init. [-1, 1]
action space: ~300
adding a baseline slightly increases the training speed
sampling may +(imitation learning with a probability to randomly sample)





