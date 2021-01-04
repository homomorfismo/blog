---
layout: post
title: Trivial but insightful
---

Today I have wasted quite some time on an exercise that was absolutely trivial but, on the bright side, I have understood the *why*  in a more meaningful way. It was exercise 1 in Chapter 1.1 of Hatcher's famous book [Algebraic Topology](http://pi.math.cornell.edu/~hatcher/AT/ATpage.html), I am revising for my February exam in case you wonder.

> **Exercise 1:**  Show that composition of paths satisfies the following cancellation property: If $f_0 \ast  g_0 \sim_c f_1 \ast g_1$ and $g_0 \sim_c g_1$ then $f_0 \sim_c f_1$.

The solution is easy: since $g_0\sim_cg_1$, we also have that $f_0\ast g_0 \sim_cf_0\ast g_1 $ just by multiplying $f(s)\ast G(t,s)$ where $G$ is the path homotopy between the $g_i$. By hypothesis we have that $f_0\ast g_0\sim_cf_1\ast g_1$, which together with the previous relation implies $f_1\ast g_1\sim_cf_0\ast g_1$. Multiplying by the inverse path $\bar g_1$ we arrive to $f_1 \sim_c f_0$.

Nonetheless, this was all symbolic computation. After concluding, I feel that there is a lack of understanding of the reasons why this must true. So let's redo the exercise, in a different fashion.

Starting with $g_0\sim_cg_1$, we know that there exists a path homotopy $(t,s)\mapsto G(t,s)$ between the two, in particular, $g_0$ and $g_1$ have the same ends $x_1,x_2$. Since $f_0\ast g_0\sim_cf_1\ast g_1$, there is a second path homotopy $(t,s)\mapsto H(t,s)$, in particular, the origin of $f_0$ and $f_1$ is the same, let's call it $x_0$, and both ends must be equal as well, since $f_i$ connects to $g_i$ for $i=0,1$, and both $g_i$ have same ends.

<img src="/pictures/2021-01-02-trivial but insightful/diagram-1-light@2x.png" class="pic_lightmode">
<img src="/pictures/2021-01-02-trivial but insightful/diagram-1-dark@2x.png" class="pic_darkmode">

The thing is that we don't know whether the paths $G_t$ pass through $x_1$ for all time $t$. It is not necessary, $f_0\ast g_0$ and $f_1\ast g_1$ are two paths  what happen to pass through $x_1$, but the path homotopy is only required to fix both ends, nothing about the rest of the path is said, it could do something like this:

<img src="/pictures/2021-01-02-trivial but insightful/diagram-2-light@2x.png" class="pic_lightmode">
<img src="/pictures/2021-01-02-trivial but insightful/diagram-2-dark@2x.png" class="pic_darkmode">

What we do know is that $G(0,\frac{1}{2}) = g_0(0) = x_1=g_1(0)=G(1,\frac{1}{2})$, and consequently we could think about this path: $j(t)=G(t,\frac{1}{2})$, which is in fact a loop by the previous observation. With this loop, we can form the map


$$
J(t,s) = \begin{cases}
G(t,s) & 0\le s \le \frac{1}{2}\\
j(t +(2s-1)(1-t)) & \frac{1}{2} \le s \le 1
\end{cases}
$$


which is an homotopy between $f_0\ast j$ and $f_1$: we have defined it so that at $s=\frac{1}{2}$ both pieces agree for any $t$ so it is continuous because the two pieces are, the levels are


$$
J(0,s) = \begin{cases}
G(0,s)= f_0(s) & 0\le s \le \frac{1}{2}\\
j(2s-1) & \frac{1}{2} \le s \le 1
\end{cases}
=f_0\ast j(s)
$$


and


$$
J(1,s) = \begin{cases}
G(1,s)= f_1(s) & 0\le s \le \frac{1}{2}\\
j(1) = x_1 & \frac{1}{2} \le s \le 1
\end{cases}
=f_1(s)
$$


as promised, and finally $J(t,0) = G(t,0)=x_0$ and $G(t,1) =j(1)=x_1 $ for all $t$, so it fixes both ends.

The thing we must show to prove the result is that $j$ is homotopic to the constant path $c_{x_1}$. And to see why, let's look at a counter example were this property is not verified. Consider the torus, the product $f_0\ast g_0$ and the loop $j$ with base $x_1$ that goes around as in the figure.

<img src="/pictures/2021-01-02-trivial but insightful/diagram-3-light@2x.png" class="pic_lightmode">
<img src="/pictures/2021-01-02-trivial but insightful/diagram-3-dark@2x.png" class="pic_darkmode">

This can be represented by the following diagram, where $j$ surrounds a hole.

<img src="/pictures/2021-01-02-trivial but insightful/diagram-4-light@2x.png" class="pic_lightmode">
<img src="/pictures/2021-01-02-trivial but insightful/diagram-4-dark@2x.png" class="pic_darkmode">

In this situation, the homotopy $G$ that induces $j$ cannot in fact be a homotopy, there is no way in which we can continuously deform $f_0\ast g_0$  into $f_1\ast g_1$ in such a way that one point goes around the whole following $j$, while having the ends $x_0$ and $x_1$ fixed.

<img src="/pictures/2021-01-02-trivial but insightful/diagram-5-light@2x.png" class="pic_lightmode">
<img src="/pictures/2021-01-02-trivial but insightful/diagram-5-dark@2x.png" class="pic_darkmode">
