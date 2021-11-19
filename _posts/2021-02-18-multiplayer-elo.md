---
layout: post
title: From pairwise comparisons to a multiplayer Elo
---

When developing a multiplayer [Elo rating system](https://en.wikipedia.org/wiki/Elo_rating_system), combining pairwise comparisons is most natural idea.  It turns out that it doesn't work so nicely if one does it naively; here I talk about how one can do it better with the help of Analytic Hierarchy Process. The rest of the post assumes familiarity with the Elo system.

[In this post](https://www.gautamnarula.com/rating/), Gautam Narula points out that unifying head-to-head comparisons overestimates or underestimates the difficulty of the clash in a basic set-up as is a match of 4 people of equal rating, therefore it can't be a good option for a rating system.  

## Analytic Hierarchy Process

Suppose there are $n$ tasks to do and we are asked to assign absolute priorities to this task. It is easy for anyone to compare two of them and say which is more important, but obtaining an ordered list may not be that easy. In fact, there is no guarantee that pairwise comparisons will be consistent so that there is a correct order of all elements that satisfy those 1 to 1 preferences. This is where Analytic Hierarchy Process (AHP) comes in.

Let's assume that we already have absolute priorities for the said $n$ tasks given by a *priority vector* $v=(v_1,\dots,v_n)$ whose components express the relative importance of each of the task to the total. Then $v$ has positive coordinates, and dividing by $\sum_i v_i$ if needed, we can assume that $v_i<1$ for all $i=1,\dots,n$ and that $\sum_i v_i = 1$. We can define the map $J(v) = (v_1^{-1},\dots,v_n^{-1})$, and consider the *preference matrix*


$$
vJ(v)^T = \left(\frac{v_i}{v_j}\right)_{i,j=1,\dots,n}
$$

whose entries are precisely the pairwise comparisons: how much more important is element $i$ than element $j$.

Now we want to do the opposite: from a matrix of pairwise comparisons we wish to obtain its priority vector, if it has one. We say that a matrix $M$ is *consistent* if there is a vector $v$ such that $vJ(v)^T = M$. Given the set of elements to rank, form the preference matrix $A=(a_{ij})$. Obviously,  $a_{ij} = a_{ji}^{-1}$ and therefore the diagonal of this matrix is composed solely of $1$'s. We say that the matrix is *reciprocal*. But there are reciprocal matrices that are inconsistent, for example this one:



$$
\begin{pmatrix}
1&2&4\\
\frac{1}{2}&1&3\\
\frac{1}{4}&\frac{1}{3}&1
\end{pmatrix}
$$



In case the matrix is consistent, then any column is a priority vector and we are done. 

> **Theorem**:  A matrix $M=(m_{ij})$ is consistent if and only if $m_{ij}m_{jk}=m_{ik}$ for any $i,j,k$.



## Multiplayer Elo and Analytic Hierarchical Process

Suppose we have players $\\{1,2,\dots,n\\}$ and we have computed the head-to-head expected scores using the Elo formula. We arrange them into a matrix such as this one



$$
\begin{pmatrix}
\frac{1}{2}&E_{12}&\dots&E_{1n}\\
E_{21}&\frac{1}{2}&\dots &E_{2n}\\
\vdots&\vdots&\ddots&\vdots\\
E_{n1}&E_{n2}&\dots &\frac{1}{2}
\end{pmatrix}
$$



The $\frac{1}{2}$ in the diagonal is just because the expected score $E_{ii}$ is effectively the one for two players of equal rating. We cannot use this matrix for AHP, we must make the quotients $\frac{E_{ij}}{E_{ji}}$, obtaining



$$
M_E = 
\begin{pmatrix}
1&\frac{E_{12}}{E_{21}}&\dots&\frac{E_{1n}}{E_{n1}}\\
\frac{E_{21}}{E_{12}}&1&\dots &\frac{E_{2n}}{E_{n2}}\\
\vdots&\vdots&\ddots&\vdots\\
\frac{E_{n1}}{E_{1n}}&\frac{E_{n2}}{E_{2n}}&\dots&1
\end{pmatrix}
$$



This is wat we called the preference matrix of our problem, and it turns out that $M_E$ is always consistent. Indeed, the entries of our matrix are of the form



$$
\frac{E_{ij}}{E_{ji}} =\frac{E_{ij}}{1-E_{ij}} = \frac{(1+10^{K(R_j-R_i)})^{-1}}{1-(1+10^{K(R_j-R_i)})^{-1}} = 10^{K(R_i-R_j)}
$$



and therefore, multiplying any two entries gives



$$
\frac{E_{ij}}{E_{ji}}\cdot\frac{E_{jk}}{E_{kj}} = 10^{K(R_i-R_j)} 10^{K(R_j-R_k)} =  10^{K(R_k-R_i)} = \frac{E_{ik}}{E_{ki}}
$$



as demanded by the theorem. Therefore, we can use any of the columns of $M_E$ as priority vector, and dividing by the sum of its coordinates we obtain the *probability of winning* for every player.

# Horse Race Problem

This is not enough for our purpose of developing a multiplayer Elo. The nicest property of the Elo system is that it allows for updating the ratings. To do that, we need to calculate the probabilities for every position of every player, and then we would judge whether the player performed better than expected or worse. What is the probability of player $j$ to finish $n$-th or worse? If it is very likely to finish in those positions but they do better, we should reward them. Otherwise, we should either punish them or leave them as their rating as it is.

The problem of estimating this probabilities based only on the odds of winning is called the Horse Race Problem (HRP) and it is not an easy one, but there are multiple approaches to solving it. Therefore once we have calculated the priority vector of the AHP problem, we could use one of the methods to solve the problem.
