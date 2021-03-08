---
layout: post
title: Not a catenary! Suspension bridge
---

The catenary problem is a well-known introduction to calculus of variations. The goal is finding the shape that a rope assumes when it hangs under its own weight (with both ends anchored), and it turns out that it is not a parabola, contrary to what most people think, although the precise curve (the graph of $\cosh x$) is very similar. Of course, as always, the problem is idealized: the rope has uniform mass density, it is inextensible, and is 1-dimensional. 

Let us ask a similar question. What is the shape of the cable of a suspension bridge, like the Golden Gate Bridge? More precisely, **what shape do we get when we hang a mass from a cable in a way that the weight is uniformly distributed along the cable?** 

## Differences with the catenary problem
The difference between this question and that of the catenary is very easily missed and it appears that we are posing the same problem. After all, isn't the weight of a cable with uniform mass density uniformly distributed along itself? Well, there's catch. Think about it this way: *what forces act upon a given point in the cable in each case?*  In the case of the catenary, a point has no mass and so the only force acting on it is the tension caused by supporting the weight of the rest of the cable. On the other hand, when there's a deck hanging off the cable, every point has two forces: the same tension as before and the due fraction of the weight of deck. As with the catenary problem, though, we will the one simplification: we will consider a massless cable, because its weight is negligible compared to that of the deck. This does not change the distinction previously made, there is effectively a downward force applied to each point in the cable, unlike in the catenary problem.

Back in December I came up with a delightfully simple proof that the shape the cable describes is in this case, funnily enough, **a parabola**. 

## Suspension bridge problem

We start by discretizing the problem. Instead of considering a smooth cable, we will approximate it by rigid segments joint at the ends, which we will refer to as **nodes**. We will deduce some formulas and then take the limit for and infinite number of infinitely short segments. Our goal is to arrive to a **differential equation** that characterizes the curve.

For each node there is a free body diagram.  As we noted in the previous section, there is a weight that each node supports, and it is the same for all of them so we denote it with $P$. Then there are tensions $T_n$ that are equal at each end of the segment, so that is the existing relation between nodes. Finally we'll need to consider the angles $\alpha_n$ that each segment makes with the vertical in order to decompose forces.

<img src="/pictures/2021-03-08-Not-a-catenary/diagrama_fuerzas_light.png" class="pic_lightmode">
<img src="/pictures/2021-03-08-Not-a-catenary/diagrama_fuerzas_dark.png" class="pic_darkmode">

Important remark: you'll see that I chose $\alpha_0 = \frac{\pi}{2}$ and that is certainly arbitrary. The easiest justification for this choice is that it does not matter. When taking the limit, that angle becomes both meaningless and irrelevant. Nonetheless, it is a very useful choice for the computations in the finite case, as we will see.

We are assuming that our bridge is in equilibrium, so let us obtain the equations for equilibrium: the sum of forces in the vertical axis must be zero, and the same goes for the horizontal axis. The general equations are


$$
\begin{cases}
T_{n+1} \cos\alpha_{n+1}= P+T_n\cos \alpha_n &(i)\\
T_{n+1}\sin\alpha_{n+1} = T_n\sin\alpha_n &(ii)
\end{cases}
$$


For $n=0$, since $\alpha_0 = \frac{\pi}{2}$, they simplify to


$$
\begin{cases}
T_1 \cos \alpha_1 = P\\
T_1 \sin \alpha_1 = T_0
\end{cases}
$$


Iterating $(i)$ we find that for all $n\in \mathbb N$



$T_{n+1} \cos \alpha_{n+1} =$  $P+T_{n} \cos \alpha_{n} =$ $2P + T_{n-1} \cos \alpha_{n-1} =$ $\ldots (n+1) P + T_0\cos\alpha_0 =$ $(n+1)P$

or equivalently, 


$$
T_n \cos \alpha_n = nP \qquad (iii)
$$


In the same way, iterating $(ii)$ it is clear that $T_{n+1}\sin \alpha_{n+1} =$ $T_{n}\sin \alpha_{n} =$ $T_{n-1}\sin \alpha_{n-1} =$ $\ldots =T_0$, that is


$$
T_{n}\sin \alpha_{n} = T_0 \qquad (iv)
$$


Let us divide $(iv)$ by $(iii)$, this way we get rid of the unknown $T_n$ and we are left with $\tan \alpha_n = \frac{T_0}{nP}$. Even better, if we multiply both side by $n$, the righthand side becomes a constant (the best thing for differential equations):


$$
n\tan \alpha_n = \frac{T_0}{P} \equiv k \qquad
$$


Before we go any further there is a small modification to make. The angle we chose was good for our diagram, but not so much for the next step. We hope to obtain a differential equation we can solve, and we know that tangents are precisely derivatives, but the correct angle is the complementary: $\beta_n = \frac{\pi}{2}-\alpha_n$.  With this change of variable, the last equation becomes


$$
n = k \tan \beta_n
$$

When taking the limit, we can identify the index $n$ of the nodes with the $x$ coordinate of the graph of a function $y(x)$, and the tangent of the angle  $\beta_n$ is no other thing but the tangent of that function at the node, that is the derivative $y'(x)$. What's the ODE then?


$$
x = k\,y'(x)
$$


and the general solution is trivially


$$
y(x) = \frac{1}{2k}x^2+C
$$


a parabola, indeed.