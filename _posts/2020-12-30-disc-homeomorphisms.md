---
layout: post
title: Disc homeomorphisms
---

Some months ago in my Algebraic Topology class a friend and I were trying to come up with an explicit homeomorphism of a punctured disc into a different punctured disc, that is, $f:\mathbb D^2\setminus \\{x_1\\} \to \mathbb D^2\setminus \\{x_2\\}$ where $x_1\neq x_2$. It was obvious that the two were homeomorphic: just think of a plate filled with honey, fix a point by placing a breadcrumb on the surface of the honey, and tilt the plate so that the honey moves around. That's the homeomorphism. But as obvious as it might be, we could not find the explicit formula.

Before moving on, I'd like to say that there is a very easy way of giving a homeomorphism, but it requires knowledge of Complex Analysis (not much) and we didn't want any extra machinery for something that we felt should be very simple. The way to do it with Complex Analysis is via a [Möbius transformation](https://en.wikipedia.org/wiki/Möbius_transformation), which is in fact a far more strict type of function than a homeomorphism, it's holomorphic.

## Simplicial homeomorphisms

A *simplex* (plural *simplices*) is a generalization of the idea of triangle. A triangle can be thought of as the *convex hull* of its vertices, that is, all the points $x$ of the form


$$
x =\sum_{i=1}^3 \lambda_iv_i
$$

where $v_1,v_2,v_3$ are the vertices of the triangle (in any $\mathbb R^n$, in fact), and $\sum_{i=1}^3\lambda_i=1$ with $0 \le \lambda_i \le 1$. In the same way, if we are given points $\\{v_0,\dots,v_n\\}$ in a space $\mathbb R^m$ with $m\ge n$ and those points verify that the vectors $v_i-v_0$ are all linearly independent (a condition which is usually abbreviated by saying that $\{v_0,\dots,v_n\}$ are in *general position*), then we can take the convex hull of those points and we call it an $n$-simplex:

$S$  $=$ $\langle v_0,\dots,v_n\mathbb \rangle$ $=$ $\left \\{ \sum_{i=0}^n\lambda_iv_i\bigg\vert \; 0\le \lambda_i\le1, \sum_{i=0}^n\lambda_i=1 \right \\}$


Now suppose that we are given two simplices with the same number of vertices, say $S_1= \langle v_0,\dots,v_n\rangle$ and $S_2= \langle w_0,\dots,w_n\rangle$, and a bijection between the vertices $f(v_i)=w_j$. We can extend $f$ to a homeomorphism of the two simplices just by sending


$$
x=\sum_{i=0}^n\lambda_iv_i\mapsto F(x) =\sum_{i=0}^n\lambda_if(v_i)
$$


we call this a *simplicial homeomorphism*.

You can check [this Geogebra applet](https://www.geogebra.org/calculator/j29tj6a9) for the $n=3$ case.

## Homeomorphism between punctured discs

Now that we now how to do this, we can construct the explicit homeomorphism we are looking for.

We consider $\mathbb D^2$, the unit disc in the plane, with a distinguished point $x_0\in \mathbb D^2$.

#### Step 1

Consider a circumscribed equilateral triangle $\mathbb T$ (not only the outline, the interior as well), which is homeomorphic to $\mathbb D^2$ by the following homeomorphism. For each $x\in \mathbb D^2$ trace the segment whose initial point is the centre of $\mathbb D^2$, passes through $x$, and ends in $y\in\partial \mathbb T$. Since the radius is $1$, the length $\Vert y \Vert$ is the ratio of the homothecy $H(x) = \Vert y\Vert x$ that stretches the radius in such way that $H(x)\in\partial\mathbb T$ for every $x\in\partial\mathbb D^2$. This is a homeomorphism because given the triangle $\mathbb T$ we can construct the inscribed circumference, and thus we can define $H^{-1}$: for any $x\in \mathbb T$, we draw the same segment as before, with endpoint $y\in \partial \mathbb T$ and then $H^{-1}(x)=\frac{1}{\Vert y \Vert} x$.

Both $H, H^{-1}$ are continuous functions and, since they are built the same way, we only need prove it for one of them. Let $\varepsilon>0$, and pick a point $x\in \mathbb D^2$, then the associated point $u\in\partial \mathbb T$ is fixed. We want to estimate for another $y\in \mathbb D^2$ (with associated point $v\in\partial\mathbb T$) the distance between their images:

$\| H(x)-H(y)\|$ $=$ $\big\| \| u\| x-\| v\| y\big\|$ $=$ $\bigg\|\| u \| x- \| v \| x + \| v \| x -\|v\| y\bigg\|$ $\le$ $\bigg\|\|u\|-\|v\|\bigg\|\cdot\|x\|+\|v\|\cdot\|x-y\|$ $\le$
$\|u - v\|\cdot\|x\|+\|v\|\cdot\|x-y\|$ $=$ $\|u - v\|\cdot\|x\|+\bigg\|\|v\|-\|u\|\bigg\|\cdot\|x-y\| + \|u\|\cdot\|x-y\|$ $\le$ $\|u -v\|\cdot\|x\|+\|u-v\|\cdot\|x-y\| + \|u\|\cdot\|x-y\|$


it is clear that we can choose a $1>\delta>0$ so that  $\|u-v\|<\min\{\frac{\varepsilon}{3\|x\|},\frac{\varepsilon}{3}\}$  if $\|x-y\|<\delta$, and in particular we can also choose $\delta<\frac{\varepsilon}{3\|u\|}$. With all that:

$\| H(x)-H(y)\|$ $\le$ $\|u - v\|\cdot\|x\|+\|u-v\|\cdot\|x-y\| + \|u\|\cdot\|x-y\|$ $<$  $\frac{\varepsilon}{3\|x\|}\|x\|+\frac{\varepsilon}{3}1+\|u\|\frac{\varepsilon}{3\|u\|}$ $=$ $\varepsilon$

and so $H$ is continuous.

Here is a [Geogebra applet](https://www.geogebra.org/geometry/v9uwwcf4) that illustrates the idea.

#### Step 2

Enter the simplicial homeomorphisms. Subdivide $\mathbb T$ into three triangles $\mathbb T_i$ with $i=1,2,3$ by connecting $x_0$ to the vertices. We chose $\mathbb T$ to be equilateral for simplicity, but any other triangle would work all the same. Consider also the barycentric   subdivision of $\mathbb T$ in triangles $T_i$ for $i=1,2,3$. Since $\mathbb T$ is equilateral the barycentre is the centre of $\mathbb D^2$. There are simplicial homeomorphisms $F_i$ that take $\mathbb T_i$ to $T_i$ for $i=1,2,3$ as we showed above. In particular, these homeomorphisms all move $H(x_0)$ to $0$.  And they glue together nicely, because in particular, all of them move points that are "close to the edges" in $\mathbb T_i$ to points "close to the edges" in $T_i$ for each $i=1,2,3$. This way we can define a global homeomorphism $F:\mathbb T\to \mathbb T$ that takes $H(x_0)$ to $0$.

More rigorously, choose a point $x_1\in \mathbb T_1$ next to an edge shared by $\mathbb T_1$ and $\mathbb T_2$, and let $\varepsilon >0$  and $x_2 \in \mathbb T_2$. For $i=1,2$ we have $F(x_i)\in T_i$, and we can draw a line connecting the images that intersects the edge shared by $T_1$ and $T_2$ at $z$. Since $F_i$ is continuous at $z$ there is $\delta>0$ such that $\|F_i(x_i)-z\|<\frac{\varepsilon}2$ if $\|x_i-H^{-1}(z)\|<\delta$ and thus


$\|F(x_1)- F(x_2)\|$ $=$ $\|F_1(x_1)- F_2(x_2)\|$ $\le$ $\|F_1(x_1)-z\|+\|z-F_2(x_2)\|$ $<$ $\varepsilon$

Since $F^{-1}$ is practically the same as $F$, the same argument is valid for the inverses.  

One more [Geogebra applet](https://www.geogebra.org/geometry/eaeefq7r) to see this in action.

#### Step 3

Use $H^{-1}$ to go back from $\mathbb T$ to $\mathbb D^2$, taking into account that $H^{-1}$ will fix $0$. The composition $H^{-1}\circ F\circ H:\mathbb D^2\to\mathbb D^2$ is the homeomorphism we were looking for.

Since we have shown that we can move every point to the origin homeomorphically, all punctured discs are homeomorphic (we have shown it for the unit disc, but translating and scaling are homeomorphisms, so this works for any two discs).



## Thoughts

As it always happens, it is easier said than proven. When I started writing this proof I thought I would take me shorter than it has, mainly because I was not going to prove rigorously the continuity of any of the functions, and $H$ has surprised me because I thought it behaved better. When I was making the Geogebra applet, I wasn't able to find $\delta$ explicitly so as to display the $B_\delta(x)$ ball surrounding the point $x$ where one can place the other point $y$ and the image of the latter falls inside $B_\varepsilon(H(x))$; and by now I still haven't found it.
