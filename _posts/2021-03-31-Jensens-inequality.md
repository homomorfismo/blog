---
layout: post
title: "Convex functions and Jensen's Inequality"
---


Suppose $\Phi:(\alpha,\beta)\subset \mathbb{R} \to \mathbb{R}$ is a convex function. By definition this means that, for any $x,y \in (\alpha,\beta)$, and $t\in [0,1]$, the following inequality holds


$$
\Phi(tx+(1-t)y) \leq t\Phi(x)+(1-t)\Phi(y)
$$


The funny thing is that for a convex combination of any $a_1, \ldots, a_r \in (\alpha, \beta)$, the analogue inequality  is also true


$$
\Phi\left(\sum_{i=1}^r a_i p_i\right) \leq \sum_{i=1}^r p_i \Phi(a_i)
$$


where, $p_i\geq 0$ for al $i=1,\dots, r$ and $\sum_{i=1}^r p_i = 1$. 

I found a completely geometric proof of this fact while trying to prove **Jensen's inequality**


$$
\Phi(E(X)) \leq E(\Phi(X))
$$


where $X$ is a random variable and $\Phi$ a convex function. 

Let's start by drawing a diagram to make things clear

<img src="/pictures/2021-03-31-Jensens-inequality/graph_light.png" class="pic_lightmode">
<img src="/pictures/2021-03-31-Jensens-inequality/graph_dark.png" class="pic_darkmode">

Consider the polygon that is formed when joining the $P_i = (a_i, \Phi(a_i))$. It must be a convex polygon, because if $P_{i_0}$ were contained in the convex hull of the others, since every $P_i$ lies on the graph of $\Phi$, that would mean that the secant line between two $P_i$ lies below a part of the graph, contrary to the condition that $\Phi$ is convex. This implies as well that the whole polygon is above the graph.

We take the convex combination of the points with the same coefficients as we had for their $a_i$'s, that is


$$
\begin{align*}
P&=\sum_{i=1}^rp_i P_i\\
 &= \sum_{i=1}^rp_i(a_i,\Phi(a_i))\\
 &=\left(\sum_{i=1}^rp_ia_i,\sum_{i=1}^rp_i\Phi(a_i)\right)
\end{align*}
$$


This is a convex linear combination of the vertices of a convex hull, so $P$​ is inside the polygon. Its $x$​-coordinate is precisely the convex linear combination of the $a_i$​ we were considering. On the other hand, the point $Q=\left(\sum_{i=1}^ra_ip_i,\Phi(\sum_{i=1}^ra_i p_i)\right)$​ lies on the graph of $\Phi$​, and since the polygon is above the graph and both $P$​ and $Q$​ lie on the line $x=\sum_{i=1}^ra_ip_i$​, we conclude that 

$$
\Phi\left(\sum_{i=1}^ra_ip_i\right) \leq \sum_{i=1}^rp_i \Phi(a_i)
$$


This (and basic measure theory) is all that's needed to prove Jensen's inequality.



### Jensen's inequality

Let $Y:\Omega \to\mathbb{R}$, be a simple non negative random variable expressed in canonical form $Y= \sum_{i=1}^r a_i \chi_{A_i}$. This means that $a_i \geq 0$ for al $i$,  $\bigcup_i A_i = \Omega$, $A_i\cap A_j = \varnothing$ if $i\neq j$, and $a_i \neq a_j$ if $i\neq j$. By definition, its expected value is


$$
E(Y) = \sum_{i=1}^r a_i P(A_i) = \sum_{i=1}^r a_i p_i
$$


this is a convex linear combination of the $a_i$. For any function $\Phi:\mathbb{R}\to\mathbb{R}$ we have that


$$
\Phi(Y) = \sum_{i=1}^r \Phi(a_i) \chi_{A_i}
$$


because for any $\omega \in \Omega$ we have $\Phi(Y(\omega)) = \Phi(a_i)$ if and only if $\omega \in A_i$. And, what is the expected value for this new random variable? It is again a simple random variable so by definition


$$
E(\Phi(Y)) = \sum_{i=1}^r \Phi(a_i) P(A_i) =\sum_{i=1}^r \Phi(a_i) p_i
$$

Finally, if $\Phi$​ is convex, we apply the inequality that we just proved
$$
\begin{align*}
\Phi(E(Y)) &= \Phi\left ( \sum_{i=1}^r a_i p_i\right)\\
&\leq \sum_{i=1}^rp_i \Phi(a_i) \\
&= E(\Phi(Y))
\end{align*}
$$

This is generalized to any random variable by approximation with simple random variables.