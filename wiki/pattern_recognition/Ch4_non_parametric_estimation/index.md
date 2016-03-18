---
layout: page
title: Ch4_non_parametric_estimation
math_support: mathjax
---


## density estimation

histogram: count sample number inside a region

K-nn: fix sample count $k$, volume $V$ varies

Parzen window: volume $V$ fixed, $k$ may vary

## K-nn

fix sample count $k$ in a local region, volume $V$ varies:

$$
p_n(x)=\frac{k_n/n}{V_n}, k_n=\sqrt{n}
$$

1D example figure: nearest-K samples, the smaller radius gives greater probability

## parzen window

window function, ( weight function, or can be a kernel function ) (TODO: kernel \& weight ??)

$$
\phi(u)=\begin{cases}
1 & \vert \mu_j \vert \leq 1/2, j=1, \dots, d \\
0 & otherwise
\end{cases}
$$

$k_n$ is the region sample count.

kernel function condition:

$$
\phi(x) \geq 0, \int \phi(u)du=1 \\
u=\frac{x - x_i}{h_n}
$$

common kernels:

1. sqare window $$ \phi(u)=\begin{cases}
1 & \vert \mu_j \vert \leq 1/2, j=1, \dots, d \\
0 & otherwise
\end{cases} $$
2. gaussian kernel 
3. exponential window
4. hypersphere kernel

proof of convergence:

$$
\begin{align}
\bar p_n(x) &= E[p_n(x)] \\
  &=\frac{1}{n} \sum_i E[\frac{1}{V_n} \phi(u)] \\
  &=\int \frac{1}{V_n} \phi(\frac{x-v}{h_n})p(v)dv \\
  &=\int \sigma_n(x-v)p(v) dv
\end{align}
$$

$h_n$ selection (meanshift algorithm)


















