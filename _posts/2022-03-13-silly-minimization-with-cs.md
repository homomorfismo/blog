---
layout: post
title: Solving a minimization problem using Cauchy-Schwarz
---



Consider a problem of the form



$$
\begin{align*}
&\min &&f(x_1,\dots, x_N)=\sum_{k=1}^N\frac{a_k}{x_k}\\
&\text{s.a.} &&g(x_1,\dots, x_N)=\sum_{k=1}^Nx_kb_k = c
\end{align*}
$$



The idea is that minimizing the summation is equivalent to minimizing it times any constant, in particular $n$. Therefore, the problem is equal to minimizing



$$
\sum_{k=1}^N\frac{a_k}{x_k}\sum_{k=1}^Nx_kb_k
$$



We can apply the Cauchy-Schwarz inequality to the vectors $({\sqrt{\frac{a_1}{x_1}},\dots,\sqrt{\frac{a_N}{x_N}}})$ y $({\sqrt{x_1b_k}}, \dots, {\sqrt{x_Nb_N}})$ dice que



$$
\sum_{k=1}^N\frac{a_k}{x_k}\sum_{k=1}^Nx_kb_k\geq\left( \sum_{k=1}^N \sqrt{a_kb_k}\right)^2
$$



which gives a lower bound. Besides, we know that the inequality an equality if and only if the two vectors are proportional, i.e. $\sqrt{\frac{a_k}{b_k}} = \lambda x_k$ for some constant $\lambda$ and for all $k = 1,\dots, N$. 



We find the value of $\lambda$ imposing the condition



$$
c = \sum_{k=1}^Nx_k = \frac{1}{\lambda}\sum_{k=1}^N \sqrt{\frac{a_k}{b_k}}
$$



resulting in



$$
x_k =c\sqrt{\frac{a_k}{b_k}}\left(\sum_{j=1}^N \sqrt{\frac{a_j}{b_j}}\right)^{-1}
$$



for all $k=1\dots, N$.