The other day a friend of mine asked me if I thought it was possible to develop a [ELO rating system](https://en.wikipedia.org/wiki/Elo_rating_system) for a British Parliamentary debate circuit. I thought I could come up with something so I gave it a go. I have been reading about the original Elo rating system (next up), [Microsoft TrueSkill](https://www.microsoft.com/en-us/research/project/trueskill-ranking-system/) rating system , and some other stuff (like [this paper](https://arxiv.org/abs/2101.00400) that came out *just about now*). I have fallen in love with the idea behind TrueSkill, and it has given me an incentive to further my study of statistical inference and modelling. 

When one first thinks about the problem, combining pairwise comparisons is the immediate idea that they want to test. As I had done some digging, I knew that it did not work (see [this post](https://www.gautamnarula.com/rating/)). But lucky for me, I read an article[^gaceta] about a year ago that was just perfect for this problem, and leads to an easy generalization of the Elo system for a specific type of set-up: games for $n$ players and just $1$ winner are subject to this type of model. The idea is an immediate mixture of two ingredients: the original Elo rating system and a piece of mathematics called Analytic Hierarchy Process (AHP). Needless to say, this does not solve the problem my friend posed me, because a BP debate is a $4$ player competition with $4$ results, but that will come (maybe) in its own time.

The post is broken into three parts: 1) an introduction to the Elo rating, 2) an introduction to AHP, 3) multiplayer Elo.

## Intro to the Elo rating system

This explanation is drawn from [this video](https://www.youtube.com/watch?v=AsYfbmp0To0) by James Grime. The basic idea is that a player's performance is not the same in every game. Their strength is subject to variability and so we resort to statistics and probability to model that strength. The Elo rating system assumes that the strength of a player follows a normal distribution whose mean we call the *Elo rating* of the player. We are ditching the details on the statistical part, because it is mostly irrelevant in the end. 

The system is designed so that the rating serves as a predictor of the outcome of a given game. If player $A$ has a rating $R_A$ and player $B$ has a rating $R_B$, we would like the difference $R_A-R_B$ to be informative of the likelihood of $A$ winning the game. The standard system is devised so that a difference of $R_A-R_B = 400$ points means that player $A$ is $10$ times more likely to win the game. If $P(A)$ is the probability of $A$ winning, we arrive at the following equation



$$
P(A)  =10^\frac{R_A-R_B}{400}P(B) = 10^\frac{R_A-R_B}{400}(1-P(A))
$$



which, solving for $P(A)$, becomes the logistic curve in the variable $x = R_A-R_B$ 



$$
P(A) = \frac{1}{1+10^{-\frac{R_A-R_B}{400}}}
$$



If we assign points for winning and losing games, for example $1$ point for winning, $\frac{1}{2}$ for a tie, and $0$ for losing, we can interpret this probability as the *expected score*, which is why it is usually denoted by $E_A$. In that sense, a expected score $E_A = 0.75$ would translate into $A$ winning or tying in amounts whose mean results in $0.75$.

Next step is relating the expected score to the *actual score* of the game. Since the expected score reflects the strength of the players relative to each other, if player $A$ wins against player $B$ while $R_A<R_B$ they are performing better than expected and maybe the rating of $A$ needs updating. There are a number of ways of interpreting this. One of them is that there is natural statistical fluctuation of the strength as we said at the beginning, in which case, successive games will hover around the value $R_A$ we started with, so the updates won't move far from it, player $A$ will keep its score more or less stable, and our predictions won't be disturbed. However, it could be that their rating may not match the *actual strength* of player $A$, which is a situation we want to take into account that they might have improved since the last time they played a rated game. 

The updating of the rating is done through the next formula, where $S_A$ is the actual score of $A$ in the game


$$
R_A' = R_A +32(S_A-E_A)
$$


We can see that the formula works as intended: if player $A$ performs better than expected, then $R_A' >R_A$, and $R_A'<R_A$ otherwise. Another feature of the formula is that it is symmetric: since $E_A+E_B = 1$, then the number of points that player $A$ adds to their rating if they win is the same that player $B$ subtracts from theirs.

The choice of $32$ as the constant for the update is completely arbitrary, but it is the original one in chess. It implies that a player can only improve or lose $32$ points maximum each time their rating is updated ($-1\leq S_A-E_A \leq 1$). It is also interesting to see the graph of the update function


$$
g(x) = 32\left(1-\frac{1}{1+10^{\frac{-x}{400}}}\right)
$$


that tells us how many points will player $A$ get if they win a game against player $B$, in terms of the difference in strength of the players $x = R_A-R_B$.


<img src="/pictures/2021-02-09\update_function_light.svg" class="pic_lightmode">
<img src="/pictures/2021-02-09\update_function_dark.svg" class="pic_darkmode">



If player $A$ that is rated much higher than player $B$, then it does not benefit $A$ much to play a game they will most likely win against $B$, but $B$ would increase their rating a lot if by any chance they win, which is a nice feature of the system. Therefore only by playing against people whose level is similar to yours (in terms of a few hundreds of points) are you likely to modify your rating.



## Analytic Hierarchy Process

AHP is a technique that transforms pairwise comparisons between entities into a global ranking of all of them. Sounds like it is precisely what we need and, at least partially, it is. 

Suppose there are $n$ tasks to do and we are asked to assign priorities to this task, to give a full order. It is easy for anyone to compare two of them and say which is more important, but obtaining an ordered list may not be that easy. In fact, there is no guarantee that pairwise comparisons will be consistent so that there is a correct order of all elements that satisfy those 1 to 1 preferences. This is where AHP comes in.

The basic idea is simple. Let's assume that we already have the global preferences/priorities for set of $n$ tasks. This can be represented as a *priority vector* $v=(v_1,\dots,v_n)$ whose components express the relative importance of each of the task to the total. Then $v$ has positive coordinates, and dividing by $\sum_i v_i$ if needed, we can assume that $v_i<1$ for all $i=1,\dots,n$ and that $\sum_i v_i = 1$. We can define the map $J(v) = (v_1^{-1},\dots,v_n^{-1})$, and consider the tensor product (also expressed as a matrix product) of $v$ and its image


$$
v\otimes J(v) = vJ(v)^T = \left(\frac{v_i}{v_j}\right)_{i,j=1,\dots,n}
$$


Those quotients are precisely the pairwise comparisons: how much more important is element $i$ than element $j$. So the objective is clear: from a matrix of pairwise comparisons, also known as the *preference matrix*, we wish to obtain its priority vector, if it has one. We say that a matrix $M$ is *consistent* if there is a vector $v$ such that $vJ(v)^T = M$ (of course, if there is one such vector, then any positive multiple of $v$ satisfies the same property).

In general, the procedure is the following. Given the set of elements to rank, form the preference matrix $A=(a_{ij})$ such that $a_{ij}$ is the preference of element $i$ of the set over element $j$, expressed in terms of how many times is $i$ more relevant than $j$. Obviously,  $a_{ij} = a_{ji}^{-1}$ and therefore the diagonal of this matrix is composed solely of $1$'s. We say that the matrix is *reciprocal*. But there are reciprocal matrices that are inconsistent, for example this one:


$$
\pmatrix{
1&2&4\\
\frac{1}{2}&1&3\\
\frac{1}{4}&\frac{1}{3}&1
}
$$


When trying to find a vector satisfying the definition, one arrives at the following system without solution:


$$
\frac{v_1}{v_2} =2 \qquad\frac{v_1}{v_3}=4 \qquad \frac{v_2}{v_3}=3
$$

In case the matrix is consistent, then any column is a priority vector and we are done. In the article I referred to at the beginning[^gaceta] there are a couple of useful characterizations of consistent matrices, but the only result that we will need is that

> **Theorem:** *A matrix $M=(m_{ij})$ is consistent iff $m_{ij}m_{jk}=m_{ik}$ for any $i,j,k$.* 

There is also a theory about the obvious problem of what to do when the matrix you get is not consistent, but as you will see next, we must not worry about it.



## Multiplayer Elo and Analytic Hierarchical Process

Suppose we have players $\{1,2,\dots,n\}$ and we have computed the head-to-head expected scores using the Elo formula. We arrange them into a matrix such as this one


$$
\pmatrix{\frac{1}{2}&E_{12}&\dots&E_{1n}\\
E_{21}&\frac{1}{2}&\dots &E_{2n}\\
\vdots&\vdots&\ddots&\vdots\\
E_{n1}&E_{n2}&\dots &\frac{1}{2}}
$$


The $\frac{1}{2}$ in the diagonal is just because the expected score $E_{ii}$ is effectively the one for two teams of equal rating. We cannot use this matrix for AHP, we must make the quotients $\frac{E_{ij}}{E_{ji}}$, obtaining


$$
M_E = \pmatrix{1&\frac{E_{12}}{E_{21}}&\dots&\frac{E_{1n}}{E_{n1}}\\
\frac{E_{21}}{E_{12}}&1&\dots &\frac{E_{2n}}{E_{n2}}\\
\vdots&\vdots&\ddots&\vdots\\
\frac{E_{n1}}{E_{1n}}&\frac{E_{n2}}{E_{2n}}&\dots&1}
$$


This is the preference matrix in AHP, and in our context we may call it *expectation matrix*. If it were a consistent matrix, we could use any of the columns of $M_E$ as priority vector, and dividing by the sum of its coordinates we would obtain the expected score of each team in the debate. It makes sense to call this vector the *expectation vector* $E = (E_1,E_2,\dots,E_n)$. It turns out that $M_E$ is always consistent. 

We want to use the characterization of consistent matrices that was mentioned before. The entries of our matrix are of the form


$$
\frac{E_{ij}}{E_{ji}} =\frac{E_{ij}}{1-E_{ij}} = \frac{(1+10^{\frac{R_j-R_i}{400}})^{-1}}{1-(1+10^{\frac{R_j-R_i}{400}})^{-1}} = 10^{\frac{R_i-R_j}{400}}
$$


and therefore, multiplying any two entries gives


$$
\frac{E_{ij}}{E_{ji}}\cdot\frac{E_{jk}}{E_{kj}} = 10^{\frac{R_i-R_j}{400}} 10^{\frac{R_j-R_k}{400}} =  10^{\frac{R_k-R_i}{400}} = \frac{E_{ik}}{E_{ki}}
$$

as demanded by the theorem.

This means that just by comparing the players' ratings head-to-head we can obtain a general expected result. Furthermore, using a vector of results $S = (S_1,\dots, S_n)$ where $S_i\in\{0,1\}$ we can update the ratings in the same way that 1 to 1 Elo does. If $R = (R_1, \dots,R_n)$ are the ratings before the game is finished, the new ratings will be $R' = R+32(S-E)$ (or any other factor). 



## Thoughts

Right now I keep on trying to use this stuff to make a multiplayer Elo ranking system that supports $n$ possible positions. This evening I thought that I could consider the problem in the same way I have presented it in this post: suppose we already have an structure with the information we want, for example a matrix whose rows are priority vectors for each of the results (first row are the chances of winning, second row chances of ending second, and so on), and see if from that matrix we can could an analogous to the matrix of pairwise comparisons. We'll see if that takes me anywhere.

[^gaceta]: La Gaceta de la RSME, Vol. 22 (2019), Núm. 1, Págs 61-79













