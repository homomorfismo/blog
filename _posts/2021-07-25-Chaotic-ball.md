---
layout: post
title: Chaotic balls by @matthen2
---

This post takes a look at my attempt to replicate the following animation and some insight I gain in the process.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">isn't it  surprising that such a simple system is chaotic? Two balls start at  almost exactly the same point, but their trajectories rapidly diverge  after only a few bounces <a  href="https://t.co/POffjTukNz">pic.twitter.com/POffjTukNz</a></p>— Matt Henderson (@matthen2) <a  href="https://twitter.com/matthen2/status/1393588039742590978?ref_src=twsrc%5Etfw">May 15, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js"  charset="utf-8"></script> 



## First attempt

My approach was to define a position vector $x = (x_1, x_2)$​​​ and say


$$
\begin{cases}
x' = y & \text{ if } \Vert X\Vert ^2 \neq 1\\
x' = R(x)y & \text{ if } \Vert X \Vert^2 = 1\\
y' = (0, -g)^T &  \text{ if } \Vert X\Vert ^2 \neq 1
\end{cases}
$$


where $R(x)$​​​​​​​ is a reflection matrix w.r.t. the tangent line. 

### The reflection matrix

When the ball hits the circle ($\Vert X \Vert ^2 = 1$​​​​​​​​​ ) choose the basis​ given by $u(x) = x$​​​​ and $v(x)=(x_2,-x_1)$​​​​​​​​​​​​, then w.r.t this basis the reflection matrix is diagonal


$$
D(x) = \begin{pmatrix} -1 & 0 \\0 & 1\end{pmatrix}
$$


Now $R = PDP^{-1}$​​​ where $P=P(x)$​​​ is the matrix whose columns are the vectors $u,v$​​​. We find that $P^{-1}(x) = (x_1^2+x_2^2)^{-1}P(x)$​​​ and therefore


$$
R(x) =\frac{1}{x_1^2+x_2^2}\begin{pmatrix} x_2^2-x_1^2 &-2x_1x_2 \\ -2x_1x_2 &x_1^2-x_2^2 \end{pmatrix}
$$



See this in action in [geogebra](https://www.geogebra.org/calculator/sardxdmd).

### The code

The argument `Y` of the following function is a 4-vector with the first two components corresponding two position and the next two corresponding to speed.

```python
def ball_in_circle(Y):
    g = 9.81
    if np.isclose(Y[0]**2+Y[1]**2, 1, atol=0.01):	#maybe substitute with Y[0]**2+Y[1]**2 >= 1
        R = 1/(Y[0]**2+Y[1]**2)*np.array([[Y[1]**2 - Y[0]**2, -2*Y[0]*Y[1]], 
                                          [-2*Y[0]*Y[1], Y[0]**2 - Y[1]**2]])
        v = R @ Y[2:]
    else:
        v = Y[2:]
    a = np.array([0, -g])
    return np.concatenate([v,a])
```

Then solve the system with Runge-Kutta as shown in [this post](https://mathstache.com/2021/07/14/two-body-problem/).

This however did not work. Sometimes the ball would hit the wall and slide along the circle, others it would do other stuff. I discussed the problem with a friend of mine, José, who suggested to simulate the bounces separately. A moment later we realised that using numeric methods to solve $X'' = (0, -g)^T$ was probably the most stupid use of Runge-Kutta ever, and this anticipated the second attempt.

As to what is the problem with the code above, my conclusion is that the problem that the matrix $R(x)$ is calculated on the assumption that $\Vert X\Vert^2 = 1$​ but this is not the case since I am only requiring the ball to be close enough to the circle. Besides, finding the correct value for the tolerance was something of trial and error (sometimes the ball would pass through the wall because of its speed), and I never liked that.

## Second attempt

That same evening I watched the [Numberphile episode](https://www.youtube.com/watch?v=6z4qRhpBIyA) and, more importantly, this reply in the comments section:

> **DrBrangar**
>
> Mathematica is capable of symbolic math, not just numeric. The code he has written solves the differential equations symbolically (because they are easy equations), and so he implicitly has a symbolic function to solve for when the next impact will be. 

As I said before, it made little sense to simulate the part where it's just a parabolic shot, so I decided to use the analytical equations.



### Idea

We will split the simulation. Start with some initial conditions and use the parametric equations of a parabolic shot 


$$
\begin{equation}
\begin{cases}
x = x_0+v_{0x}t\\
y = y_0 + v_{0y}t - \frac{1}{2}gt^2
\end{cases}
\end{equation}
$$


to find the hitting time against the circle. With the parametric equations,we get the positions at every frame before the hitting time. We repeat the process with the hitting position and velocity as new initial conditions every time the ball bounces, until the animation is completed.

### Hitting time

Take the parametric equations and substitute in equation of the circle


$$
(x_0+v_{0x}t)^2+ (y_0 + v_{0y}t - 0.5gt^2)^2 = 1
$$


We expect this 4th-degree polynomial to have a single positive root, from the physical interpretation of our problem, which is precisely the hitting time.

We find this root using the `sympy` Python library, that allows for symbolic computation. In the next function `X` and `V` are the initial position and velocity

```python
def hit_time_pos_vel(X,V):
    t = symbols('t')

    # find the roots
    rts = solveset(0.25*g**2*t**4 -g*V[1]*t**3 + (-g*X[1]+V[0]**2+V[1]**2)*t**2+(X[0]*V[0]+X[1]*V[1])*2*t+X[0]**2+X[1]**2 -1, t)

    # choose the positive root (threshold because of computing errors)
    pos_rts = Intersection(rts, Interval.open(1e-12, oo))

    # convert to float
    hit = float(pos_rts.args[0])

    # find where it hits, and velocity when it does
    Y = np.array([X[0]+V[0]*hit, X[1]+V[1]*hit-0.5*g*hit**2])
    W = np.array([V[0], V[1]-g*hit])

    return hit, Y, W

```



### Simulation

As always, the code is written for any number of balls. This time I have improved the settings for the video (code under `# time thingies` ), now the speed of the animation is the appropriate and the length as well.

```python
# DEFINE BALLS AND SETTING
positions = [np.array([0,0])]
velocities = [np.array([1,0])]
n = len(positions)
c = 1  # elastic coeficient

# time thingies
T = 5       
fps = 60
N = fps*T    
times = np.linspace(0,T,N)
dt = 1/fps

# SIMULATE EACH BALL SEPARATELY
g = 9.81
balls = np.zeros((N,n,2))		# initialize the array containing the simulation data

for k in range(n):
    X = positions[k]
    V = velocities[k]
    simulate_ball(X, V)
```



### Animation

Nothing changes much from code in the previous post.

```python
# SET UP PLOT
fig, ax = plt.subplots()
ax.set_xlim(-1.2,1.2)
ax.set_ylim(-1.2,1.2)
ax.axis('equal')

# CREATE OBJECTS
circle = Circle((0,0),radius= 1, fill = False , color = 'r')
ax.add_patch(circle)
particles = [plt.plot([], [], marker='o', color=palette[k])[0] for k in range(n)]

# FRAMES
def update(i):
    for k in range(n):
        particles[k].set_data(balls[i][k][0], balls[i][k][1])
    return particles

# RENDER ANIMATION
mywriter = animation.FFMpegWriter(fps=fps)
plt.ioff()
ani = animation.FuncAnimation(fig, update, frames = range(N))
ani.save('chaotic_ball1.mp4', dpi = 600, writer = mywriter)
```



Here is the result

<iframe width="400" height="400" src="https://www.youtube.com/embed/tef2pTLeSfA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>