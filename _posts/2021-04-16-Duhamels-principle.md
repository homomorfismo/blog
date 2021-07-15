---
layout: post
title: Duhamel's principle and a funny derivative
---

Today I learnt about [Duhamel's principle](https://en.wikipedia.org/wiki/Duhamel%27s_principle), which I can't help miswriting like [Dudamel](https://www.youtube.com/watch?v=ZXeWiixwEz4). I believe it has also been the first time I find myself trying to differentiate a function such


$$
F(t) = \int_0^tf(t,s)ds
$$


And that's all it takes to prove Duhamel's principle.
<!-- excerpt-end -->

### Statement of Duhamel's principle

It is a general method for obtaining solutions to inhomogeneous linear partial equations of the form



$$
\tag{P}
\begin{cases}
u_t(x,t)+L_xu(x,t)=f(x,t)\\
u_{|\partial \Omega}(x,t) = 0\\
u(x,0) = 0
\end{cases}
$$



where $x\in\Omega, t>0$ and $L_x$ is a linear differential operator in the spatial variable. The principle says that the solution to ($(P)$) is given by $u(x,t) = \int_0^tv_s(x,t)ds$ where for each $s \in [0,t]$, the function $v_s(x,t)$ is the solution to this auxiliary problem



$$
\tag{Ps}
\begin{cases}
v_t(x,t)+L_xv(x,t)=0\\
v_{|\partial \Omega}(x,t) = 0 \\
v(x,s) = f(x,s)
\end{cases}
$$



The intuition behind it, as the Wikipedia page puts it, is that *the inhomogeneous problem can be thought of as a set of homogeneous problems each starting afresh at a time $t = t_0$. By linearity, one can add up (integrate) the resulting solutions through time $t_0$ and obtain the solution for the inhomogeneous problem*.

On the one hand, it is clear that the boundary conditions of ($(P)$) are satisfied by $u$. On the other hand, we need to show that $u$ solves the equation and for that we need to compute



$$
\tag{1}
\frac{\partial u}{\partial t}(x,t) = \frac{\partial }{\partial t}\int_0^t v_s(x,t)ds
$$



### Differentiating $F(t) = \int_0^tf(t,s)ds$

In other to differentiate this function, we will only need to bear in mind the [integral mean-value theorem](https://en.wikipedia.org/wiki/Mean_value_theorem#Mean_value_theorems_for_definite_integrals). We will work with the definition of derivative



$$
F'(t) = \lim_{h\to 0} \frac{F(t+h)-F(t)}{h}=
$$

$$
=\lim_{h\to0}\frac{1}{h}\left( \int_0^{t+h}f(t+h,s)ds-\int_0^tf(t,s)ds \right)
$$

$$
=\lim_{h\to0} \left( \int_0^{t}\frac{f(t+h,s)-f(t,s)}{h}ds\\ + \frac{1}{h}\int_t^{t+h}f(t+h,s)ds  \right)
$$



Now, by the mean value theorem  we have that


$$
\frac{1}{h}\int_t^{t+h}f(t+h,s)ds = f(t+h,\delta)
$$


for some $\delta \in [t,t+h]$. Passing to the limit when $h\to 0$ we are forced to have $f(t,t)$. Assuming that we can swap the integral and the limit, the left term is just the integral of the partial derivative with respect to $t$.

We arrive to the nice formula



$$
\tag{2}
F'(t)  = \int_0^t\frac{\partial f}{\partial t}(t,s)ds +f(t,t)
$$



#### Example

Let us exemplify it with $f(t,s) = \cos (t+s)$, and $F(t) = \int_0^t \cos(t+s)ds$. We can compute it directly and find that



$$
F(t) = \int_0^t \cos(t+s)ds = \sin(2t)-\sin(t)
$$


and therefore


$$
F'(t) = 2\cos(2t)-\cos t
$$


Now we use ($(2)$). On the one hand, $f(t,t) = \cos (2t)$. On the other, $\frac{\partial f}{\partial t}(t,s) = -\sin (t+s)$, and so


$$
\int_0^t \frac{\partial f}{\partial t}(t,s) ds= \int_0^t  -\sin (t+s)ds =
$$

$$
=\cos(2t)-\cos t
$$


putting all together we arrive at $F'(t) = 2\cos(2t)-\cos t$ again.



### Proof of Duhamel's principle

Going back to ($(1)$) and using ($(2)$) we find that


$$
\frac{\partial u}{\partial t}(x,t) = \int_0^t \frac{\partial }{\partial t}v_s(x,t)ds+v_t(x,t)
$$

and since $v_s$ is solution to ($\ref{Ps}$) then $\frac{\partial }{\partial t}v_s(x,t) = -L_xv_s(x,t)$ and $v_t(x,t) = f(x,t)$. If we assume that we can take the linear operator out of the integral, we have


$$
-L_x\int_0^t v_s(x,t)ds = -L_xu(x,t)
$$


therefore



$$
u_t(x,t) =-L_xu(x,t)+f(x,t)
$$



so naturally $u_t(x,t)+L_xu(x,t)=f(x,t)$ and $u$ solves the equation.
