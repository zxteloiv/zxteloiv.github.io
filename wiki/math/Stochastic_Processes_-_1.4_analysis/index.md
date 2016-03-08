---
layout: page
title: Stochastic_Processes_-_1.4_analysis
math_support: mathjax
---


## 1-4 随机分析
### 1.收敛(极限)
Idea: $E[(X_n-X)^2]\to 0$ 优于传统的 $P\{\vert X_n-X\vert\leqslant\varepsilon\rightarrow1\}$，后者需使用切比雪夫不等式，不太方便，前者可以用测量实验数据的方法得到。

均方收敛：给定随机过程$\{X_n,n\in\mathbb N\}$和随机变量X，若$$\lim_{n\to\infty}E[(X_n-X)^2]=0$$，称$X_n$均方收敛于X，计作$\mathsf{l.i.m}X_n=X$

性质证明需用到 Swartz 不等式：
$$
0\leqslant E(X+tY)^2=E(X^2)+2tE(XY)+t^2E(Y^2) \\
\Rightarrow \Delta=b^2-4ac\leqslant0
\Rightarrow \vert E(XY) \vert \leqslant \sqrt{E(X^2)E(Y^2)}
$$

**性质1** : 唯一性 $\mathsf{l.i.m}_{n\to\infty}X_n=X, \mathsf{l.i.m}_{n\to\infty}Y_n=Y$则$P\{X=Y\}=1$
*Hint* : $\vert E(X-Y)^2\vert \leqslant \vert E((X-X_n)+(X_n-Y))^2\leqslant\dots$

**性质2** : 极限期望可以换序 $l.i.m X_N \to X$ 则 $\lim_{n\to\infty}E(X_n)=E(l.i.mX_n)=E(X)$ 
*Hint* : $\vert E(X_n)-E(X)\vert = \vert 1*E(X_n-X)\vert \leqslant \sqrt{E(X_n-X)^2} \to 0$

**性质3** : $l.i.mX_n\to X, l.i.mY_n\to Y \Rightarrow E(X_nY_n)\to E(XY)$
*Hint* : 加一项减一项

**性质4** : $l.i.mX_n\to X, l.i.mY_n\to Y \Rightarrow  \\
l.i.m(aX_n+bY_n)=aX+bY\,a,b\in \mathbb R $
*Hint* : $E(aX_n+bY_n-(aX+bY))\to0$

**性质5** : $\lim{a_n}=a,l.i.ma_nX=aX$, X为确定的随机变量
*Hint* : $E(a_nX-aX)^2$

**性质6** : $l.i.mX_n$存在$\Leftrightarrow l.i.m(X_n-X_m)=0$
*Hint* : =>: 存在=X时,(Xn-X)-(Xm-X); <=: 子列

### 2. 连续性
随机过程$\{X(t),t\in T\}$, T 需要连续。
对 $t_0\in T$ 若 $l.i.m_{t\to t_0}X(t)=X(t_0)$ 则 $X(t)$ 在 $t=t_0$ 处连续（类似有左右连续）$\lim_{t\to t_0}E(X(t)-X(t_0))^2=0$

**例1** : $X(t)=At+B,t\in(a,b)$ 其中 A、B 的一二阶矩存在. 求证 $X(t)$ 连续.
*Hint* : $\forall t_0\in(a,b)  \\
E(X(t)-X(t_0))^2=E(A(t-t_0))^2=(t-t_0)^2E(A^2)\to0$

**Th.1** $\{X(t)\}$ 连续 $\Leftrightarrow R_X(t_1,t_2)$ 在 $\{(t,t)\vert t\in T\}$ 连续（将二元连续限制在直线上）
*Hint* :
'=>': $\lim R_X(t_1,t_2)=\lim E(X(t1)X(t2))=\dots$ (???)
'<=': $\lim_{t\to t_0} E(X(t)-X(t_0))^2 \\
= \lim_{t\to t_0}(R_X(t,t)-2R_X(t,t_0)+R_X(t_0,t_0))=0
$

### 3. 导数
$\{X(t),t\in T\}$ 且 $t_0\in T, \Delta t + t_0 \in T$.
$l.i.m_{\Delta t\to0}\frac{X(t_0+\Delta t)-X(t_0)}{\Delta t}$ 存在, 则 $X(t)$ 在 $t=t_0$ 处可导

**性质1** : 可导必连续.
*Hint* : $\lim_{\Delta t\to0}E(\frac{X(t_0+\Delta t)-X(t_0)}{\Delta t} - X'(t_0))^2=0$

**性质2** : $E(X'(t))\triangleq M_{X'}(t)=m'_X(t)$ 即求导和均值函数互换次序
*Hint* : $E(l.i.m_{\Delta t\to0}\frac{X(t+\Delta t)-X(t)}{\Delta t})) \\
=l.i.m_{\Delta t\to0}E(\frac{X(t+\Delta t)-X(t)}{\Delta t}) \\
=l.i.m_{\Delta t\to0}\frac{1}{\Delta t}(m_x(t+\Delta t) -m_x(t))
$

**性质3** : $(aX(t)+bY(t))'=aX'(t)+bY'(t)$
**性质4** : $(X)'=0$
$\star$**性质5** : $(f(t)X(t))'=f'(t)X(t)+f(t)X'(t)$
*Note* : $(X(t)Y(t))'$ 不一定存在，要求四阶矩
*Hint* : $$(f(t)X(t))'
= l.i.m_{\Delta t\to0}\frac{f(t+\Delta t)X(t+\Delta t)-f(t)X(t)}{\Delta t}
=...
$$ 加项减项

**性质6**: $R_{X'}(t_1,t_2)=\frac{\partial R_X(t_1,t_2)}{\partial t_1 \partial t_2}$
*Hint* : $$ = E(X'(t_1)X'(t_2)) \\
= E(l.i.m_{\Delta t\to0} ..., l.i.m_{\Delta k\to0}, ...) \\
= \lim\lim E(\cdots\cdots) \\
= \lim\lim\frac{1}{\Delta\Delta}(R_X(\dots)+...+...+R_X(...))
$$

**Th.2** X(t) 可导 $\Leftrightarrow \lim_{h_1,h_2\to0}\frac{(R_X(t+h_1,t+h_2)-R_X(t+h_1,t)-R_X(t,t+h_2)+R_X(t,t))}{h_1h_2}$ 存在

### 4. 均方积分
$\{X(t),t\in T\}, f(t),t\in T, t=[a,b]$
$\int_a^bf(t)X(t)dt=l.i.m_{\Delta t_i\to0}\sum_{i=1}^nf(j_i)X(j_i)\Delta t_i$ 存在

**Th3** $\int_a^b\int_a^bf(s)f(t)R_X(s,t)dsdt$ 存在 $\Rightarrow \int_a^bf(t)X(t)dt$ 存在

**性质1**:
.... omitted ....


