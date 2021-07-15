---
layout: post
title: Two-body problem in matplotlib
---

I finally got round to learn how to make animations with matplotlib and I started with the animation of the $N$-body problem.

While working on it, I came across this [post](https://towardsdatascience.com/modelling-the-three-body-problem-in-classical-mechanics-using-python-9dc270ad7767) by Gaurav Deshmukh that has a very nice animation of the Alpha Centaury binary system. I tried deriving the necessary initial velocity for that stable setup analytically in a very special case: two particles of equal mass in elliptic orbits as you can see in the next section. But there is something I'm missing because with the resulting speed I get this animation:

<iframe width='auto' src="https://www.youtube.com/embed/RLXghR9DMbE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Analysis of the 2-body problem with equal masses

Let $2R_a$ be the distance of the two masses when they are the furthest apart, so that $R_a$ is the distance to $COM$. In a gravitational field mechanical energy is conserved and, the total mechanical energy for any of the masses $m$ is


$$
E = \frac{1}{2}mv^2-\frac{Gm^2}{d}
$$


where $d$ is the distance between the two masses. Solving for $v^2$ yields

$$
\begin{equation}
v^2 = \frac{2E}{m}+\frac{2Gm}{d}
\end{equation}
$$

From the conservation of angular momentum $\textbf L = \textbf r \times m \textbf v$ we get that the modulus is constant $L = rmv \sin \theta$  where $\theta = \angle(\textbf r, \textbf v)$. Then, at the perihelion and aphelion


$$
\frac{v_a}{R_p} = \frac{v_p}{R_a}
$$


We are going to substitute $v = v_a$ in (1). The distance between the two masses when one (therefore both) are at the aphelion is $2R_a$ and so


$$
\begin{equation}
v_a^2 = \frac{2E}{m}+\frac{2Gm}{2R_a}
\end{equation}
$$


On the other hand, $v_p = v_aR_aR_p^{-1}$ and we get a second equation for $v_a^2$


$$
\begin{equation}
v_a^2 = \frac{2ER_p^2}{mR_a^2}+\frac{2GmR_p}{R_a^2}
\end{equation}
$$


From (2) and (3) we obtain the identity


$$
E\left( \frac{1}{m}-\frac{R_p^2}{mR_a^2}\right) = Gm\left( \frac{R_p}{R_a^2}-\frac{1}{2R_a} \right)
$$


which can be rewritten like

$$
\begin{equation}
E = \frac{Gm^2}{2}\frac{2 R_p - R_a}{R_a^2-R_p^2}
\end{equation}
$$

which is valid as long as $R_a \neq R_p$, where $R_a> R_p$ are free for us to choose.



### Python code for the $N$-body problem

As a good mathematician, I did not write the code for a particular case e.g. $N=2$,  but for any number of bodies. Here you have an example with $N=5$:



<iframe width='auto' src="https://www.youtube.com/embed/Gmo6t3AG3uk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



These are the things you need to import:

```python
from matplotlib import pyplot as plt
import numpy as np
from matplotlib import animation
from matplotlib.colors import BASE_COLORS as palette
```



I wrote a Python implementation for the Runge-Kutta method of order 4 because I felt like doing it, you can use e.g. the Scipy ODE solver, since I believe it uses the same parameters.

```python
def rk4auto(f, I, x0, N=1000):
    n = x0.shape[0]
    h = (I[1] - I[0])/N                 # step size
    t = np.linspace(I[0], I[1], N+1)    # times
    x = np.zeros([N+1, n])              # initialize vector solution
    x[0] = x0
    for i in range(N):
        F1 = f(x[i])
        F2 = f(x[i] + h/2*F1)
        F3 = f(x[i]+h/2*F2)
        F4 = f(x[i]+h*F3)
        x[i+1] =  x[i] + h/6*(F1+2*F2+2*F3+F4)
    return x
```



In our case, we have a system of $N$ linear, second order, autonomous ODEs $\textbf{X}_i^{\prime\prime} = F_i(\textbf{X}_1, \dots, \textbf{X}_N)$ governed by the law of gravitation


$$
F_i(\textbf X_1, \dots, \textbf X_N) = G\sum_{j\neq i}\frac{m_j}{\|\textbf{X}_j-\textbf{X}_i\|^3}(\textbf{X}_j-\textbf{X}_i)
$$

for position vectors $\textbf{X}_i = (X\_{i1}, X\_{i2}, X\_{i3})$ for every $i=1,2,\dots, N$. In order to use a method like Runge-Kutta to solve it, we must transform it into a system of first order ODEs, so as usual define new variables $\textbf Y_i = \textbf{X}_i^\prime$, and the vector

$$
\textbf Y =(\textbf X_1, \dots, \textbf X_N, \textbf Y_1, \dots, \textbf Y_N)
$$


The solver `rk4auto` takes as an argument the function

$$
f(\textbf Y) =
\begin{pmatrix}
\textbf Y_1 \\ \vdots \\ \textbf Y_N\\  F_1(\textbf X_1, \dots, \textbf X_N) \\ \vdots \\  F_N(\textbf X_1, \dots, \textbf X_N)
\end{pmatrix}
$$

and solves the system $\textbf Y^\prime = f(\textbf Y)$. Nevertheless, since I wanted to write the code in the most general way, our function needs to be given the masses because I don't want to include any concrete data in its definition, so we append them to our vector of variables:


$$
\textbf Y =(\textbf X_1, \dots, \textbf X_N, \textbf Y_1, \dots, \textbf Y_N, m_1, \dots, m_N)
$$


This vector must be flattened because that's the way `rk4auto` expects it and the way it will return the information in the iteration scheme. It is better to do this rather than changing how `rk4auto` is defined because: 1) every solver works this way, so you get accustomed for that one day you will want to use a better solver which you won't be able to modify, 2) we will be able to use this solver for other problems.

As I comment in the code, we recover the vectors once we are inside the function.

```python
def n_body3d(Y):
    N = Y.shape[0]//7

    # RECOVER VECTORS
    X = np.array([np.array([Y[3*i], Y[3*i+1], Y[3*i+2]]) for i in range(N)])

    # CALCULATE DISTANCES (cubed)
    norms = np.zeros([N,N])
    for i in range(N):
        for j in range(i+1,N:
            norms[i,j] = norms[j, i] = (np.linalg.norm(X[j] - X[i]))**(3/2)

    # CALCULATE ACCELERATIONS
    Z = np.array([ G*np.sum(np.array([Y[6*n+j]*(X[j]-X[i])/norms[i,j] for j in range(N) if j != i]), axis=0) for i in range(N)])

    return np.concatenate([Y[3*N:6*N], Z.flatten(), Y[6*n:]])
```

Two comments: 1) Every vector has 3 components and that tells us that vector $\textbf Y$ has $2\cdot3N+N = 7N$ components, 2) it is best practice (computationally cheaper) to calculate all the $\Vert\textbf X_j -\textbf X_i\Vert$ and keep them in a matrix because this is information we are going to use over and over and there are quite a few operations involved. We could've probably done the same for $\textbf X_j - \textbf X_i$.



On to the simulation. First of all, define the bodies for the simulation, here is the code for a 5-body problem

```python
G = 1e2

masses = np.ones(5)
N = len(masses)

positions = [np.array([-2,0,0]), np.array([1,0,0]),
            np.array([1,1,0]), np.array([0,-3,0]),
            np.array([0,1,3])]

velocities = [np.array([0,0.5,-0.5]), np.array([0,0.5,0]),
            np.array([0.5,0.5,0.5]), np.array([-0.5,0.5,-0.5]),
            np.array([0.5,0,0.5])]
```



Next we solve the problem numerically

```python
r = np.concatenate(positions)
v = np.concatenate(velocities)
X0 = np.concatenate([r, v, np.array(masses)])

sol = rk4auto(n_body3d, [0,100], X0, int(1e5))
```
and separate the part of the solution corresponding to the simulation of each of the bodies. We  create an array of vectors where each element of the array is one of the bodies. Besides, we calculate the center of mass of the system because it will be useful to center the view.
```python
stars = np.array([sol[:,(3*k):(3*(k+1))] for k in range(N)])
com = np.sum(np.array([masses[j]*stars[j] for j in range(N)]),axis=0)/np.sum(masses)
```



Once we have all the necessary data, we construct the plot and the elements to be plotted, which we will later animate with the data. This code assigns a different color to every particle and trajectory automatically (up to 8 particles I believe).

```python
# PLOT
fig = plt.figure()
ax = fig.add_subplot(projection = '3d')
ax.view_init(15, 45)

# OBJECTS TO BE PLOTTED
palette = list(mcolors)
particles = np.array([plt.plot([],[], [], marker='o', color=palette[k])[0] for k in range(N)])
trajectories = np.array([plt.plot([],[], [], color=palette[k],alpha=0.5)[0] for k in range(N)])
```



Finally, define the `update` function which tells matplotlib how to update our objects every frame of the animation. This must be understood as: at frame `i` which corresponds to the i-th element of the partition we used in the simulation, the k-th particle must be at positions... and the trajectory goes from the beginning to the i-th position. Here we use the center of mass to center the plot view as I said earlier.

```python
def update(i):
    for k in range(N):
        particles[k].set_data(stars[k][:,0][i], stars[k][:,1][i])
        particles[k].set_3d_properties(stars[k][:,2][i])

        trajectories[k].set_data(stars[k][:,0][:i+1],stars[k][:,1][:i+1])   
        trajectories[k].set_3d_properties(stars[k][:,2][:i+1])

    ax.set_xlim([com[:,0][i]-1,com[:,0][i]+1])
    ax.set_ylim([com[:,1][i]-1,com[:,1][i]+1])
    ax.set_zlim([com[:,2][i]-1,com[:,2][i]+1])

    return np.concatenate([particles, trajectories])
```



The only thing left to do is rendering the animation. There are many ways to set the properties, but I find that this one is best: choose the framerate first or otherwise you could have problems with your player, and it is easier to specify the length of the animation.

```python
seconds = 20
framerate = 60
mywriter = animation.FFMpegWriter(framerate)
plt.ioff()
ani = animation.FuncAnimation(fig, update, frames = range(seconds*framerate))
ani.save('5bodyproblem.mp4', dpi=600, writer = mywriter)
```
