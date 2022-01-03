---
layout: post
title: A continuous fraction for the Golden Ratio
---



The Fibonacci sequence is defined recursively like this



$$
\begin{cases}
x_{n+2} = x_{n+1} + x_n \\
x_0 = 0, x_1 = 1
\end{cases}
$$



We can study the quotients between one Fibonacci number and the next



$$
y_n = \frac{x_n}{x_{n+1}}
$$



and notice that there is also a recursive relation that defines the sequence



$$
y_{n+1} = \frac{x_{n+1}}{x_{n+2}} = \frac{x_{n+1}}{x_{n+1}+x_n} = \frac{1}{1+y_n}
$$



We would like to prove that this sequence of quotients has a limit. The easiest way to do that is to prove that it is a monotone and bounded sequence. The latter is automatic since $0<y_n<1$ for all $n\in \N$. On the other hand, notice that



$$
\frac{y_{n+1}}{y_{n+2}} = \frac{1+y_{n+1}}{1+y_n} > 1 \iff y_{n+1} > y_n
$$



therefore the sequence is not monotone. We will have to work just a bit more.

We can consider the quotient $\frac{y_{n+1}}{y_{n+3}}$ and find another rule. The two rules together will help us study the sequence:



$$
\begin{cases}
y_{n+1} > y_{n+2} \iff y_{n} < y_{n+1}\\
y_{n+1} > y_{n+3} \iff y_{n} < y_{n+2} \tag{1}
\end{cases}
$$



The first terms of the sequence are



$$
y_0 = 0, \ y_1 = 1,\ y_2 = \frac{1}{2}, \ y_3 = \frac{2}{3}, \ y_4 = \frac{3}{5}
$$


so the ordering is


$$
y_0<y_2 < y_4 < y_3<y_1
$$



The pattern we suspect is that the subsequence of even indices $\{y_{2n}\}$ is increasing, and the subsequence of odd indices $\{y_{2n+1}\}$ is decreasing, and that each bounds the other. In fact we don't need to show that they bound each other, because as we said $0<y_n<1$, although it is true and just for completeness we will prove it later. 

Using induction, showing monotonicity is immediate: given the initial data and supposing that $y_{2n-2}<y_{2n}$ y que $y_{2n+1} < y_{2n-1}$ one can  trivially see that $y_{2n}<y_{2n+2}$ and $y_{2n+3}<y_{2n+1}$ using the formulas from $(1)$. This proves that $\{y_{2n}\}$ and $\{y_{2n+1}\}$ have a limit, let us call $L_e:=\lim_n y_{2n}$ and $L_o:=\lim_n y_{2n+1}$. 

For every $n\in \N$ even or odd we have that



$$
y_{n+2} = \frac{1}{1+y_{n+1}} = \frac{1}{1+\frac{1}{1+y_{n}}} = \frac{1+y_{n}}{2+y_{n}}
$$



Then, we can let $n\to \infty$ and find that both $L_e,L_o$ satisfy the same second order equation. This equation has only one positive root and therefore both limits coincide $L_p = L_i = L$:



$$
L = \frac{1+L}{2+L}\ \iff \ L^2+L-1 = 0 \ \iff \ L = \frac{-1\pm \sqrt{5}}{2}
$$



Additionally, we have rigorously proved one more thing, which is that this limit can be written as a continuous fraction



$$
L =  \lim_{n\to \infty}\frac{1}{1+y_n} = \frac{1}{1+\frac{1}{1+\frac{1}{1+...}}}
$$



Recalling that the golden ratio is $\varphi = \frac{1+\sqrt{5}}{2} = 1+\frac{-1+\sqrt{5}}{2} = 1+L$, we have obtain a continuous fraction for it

$$
\varphi =1+ \frac{1}{1+\frac{1}{1+\frac{1}{1+...}}}
$$





### Proof that the subsequences bound each other

First notice that $y_{2k}<y_{2k+1}$ for all $k\in \N$. Indeed 



$$
y_{2k}<y_{2k+1} \iff y_{2k-1}>y_{2k} \iff\dots\iff y_1 >y_2 \tag{2}
$$



and we know that the latter is true.

Take any $n,m\in \N$  and let us show that $y_{2n}<y_{2(n+m)+1}$. We may write the quotient like



$$
\frac{y_{2n}}{y_{2n+2m+1}} = \frac{y_{2n}}{y_{2n+2}}\frac{y_{2n+2}}{y_{2n+4}}\dots\frac{y_{2n+2m-2}}{y_{2n+2m}}\frac{y_{2n+2m}}{y_{2n+2m+1}} <\frac{y_{2n+2m}}{y_{2n+2m+1}}< 1
$$



the inequalities come from the fact that the subsequence of even terms is increasing, so all those quotients are less than $1$, and the last of them is just the observation $(2)$. 

We can similarly show the other bound.

