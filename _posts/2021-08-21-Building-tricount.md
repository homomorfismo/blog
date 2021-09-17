---
layout: post
title: Building tricount
---

Today I figured out how [Tricount](tricount.com) works. It is an integer linear programming problem whose formulation is not difficult. In the most basic version we want to tally the payments that people made for a list of shared expenses with the least number of transactions, that is, the expenses were paid by several people and the rest have to pay there share to even out the contributions. The slightly more complex version contemplates a situation where not everyone pays for every item, e.g. maybe someone didn't use item X and so it is fair that they don't pay for it.

# Basic version

Let $\mathcal{I} = \\{1,2, \ldots, n\\}$ be the set of people. The data that we know is the amount $m_i$ that each person  $i\in \mathcal I$ has already contributed. We are interested in who pays whom so the natural decision variables are binary variables $x_{ij}\in \\{0,1\\}$ for every $i,j \in \mathcal I$, where $x_{ij} = 1$ if person $i$ pays person $j$ and is $0$ otherwise. Since we are interested in minimizing the number of transactions, the obvious cost function is



$$
f(x_{11},\ldots, x_{nn}) = \sum_{i,j\in \mathcal I} x_{ij}.
$$

We will need some auxiliary variables $y_{ij}$[^1] for every $i,j \in \mathcal I$ representing the amount that person $i$ pays person $j$. There is also a relation of these variables among themselves: $y_{ij} = -y_{ji}$ for every $i,j \in \mathcal{I}$. Of course the only true transactions will be when $y_{ij}>0$, this is captured by the next *switch* type constraints that relate $x_{ij}$ and $y_{ij}$. 

Let $M=\sum_{i\in \mathcal I} m_i$, we impose that

$$
y_{ij} \leq M x_{ij} \quad\forall i,j \in \mathcal I;
$$

which is perfectly fine since $y_{ij}$ is at most $M$ and is the formalization of the condition "if person $i$ has to pay a positive amount to person $j$, then a transaction has to be made".

Lastly, the constraint that makes the whole thing work is

$$
\tag{1}
m_i + \sum_{j\in \mathcal{I}}y_{ij} = \frac{1}{n}M \quad \forall i\in \mathcal{I},
$$


which reads "what person $i$ has already paid plus what they will be paying must equal one $n$-th of the total".

The full formulation of the problem is then


$$
\begin{align*}
&\min && \sum_{i,j\in \mathcal I} x_{ij}\\
&\text{s.t.} &&  y_{ij} \leq M x_{ij} \quad\forall i,j \in \mathcal I\\
& && m_i + \sum_{j\in \mathcal{I}}y_{ij} = \frac{1}{n}M \quad \forall i\in \mathcal{I} \\
& && y_{ij} = -y_{ji} \quad \forall i,j \in \mathcal{I} \\
& && x_{ij} \in \{0,1\} \quad \forall i,j \in \mathcal{I}.
\end{align*}
$$


# Complex version

Now we are taking into account who has to pay for what expenses, so we need to keep track those: let $\mathcal{J}=\\{1,2, \ldots, l\\}$ be the set of expenses. With this new set of indexes, let $P = (p^i_j)_{i\in \mathcal{I}, j\in \mathcal{J}}$ be the matrix whose entries are $1$ if person $i$ has to pay for item $j$ and $0$ otherwise. Rows store information about what expenses a given person has to pay for, and columns store information on who is paying for a given expense. Finally, for every expense $j\in J$ we know its cost $q_j$.

Constraint $(1)$ is substituted by the following:


$$
m_i + \sum_{j\in \mathcal{I}}y_{ij} = \sum_{j\in \mathcal{J}} \frac{1}{\sum_{i\in \mathcal{I}}p^i_j}p^i_j q_j \quad \forall i\in \mathcal{I}. \tag{2}
$$


Thanks to $p^i_j$, the RHS of $(2)$ will be $0$ if they don't have to pay for the expense $j$, and otherwise it will equal the cost of that expense divided by the number of people who have to pay for it.

The rest of the variables and restrictions on them remain unchanged so that the final problem reads as follows


$$
\begin{align*}
&\min && \sum_{i,j\in \mathcal I} x_{ij}\\
&\text{s.t.} &&  y_{ij} \leq M x_{ij} \quad\forall i,j \in \mathcal I\\
& && m_i + \sum_{j\in \mathcal{I}}y_{ij} = \sum_{j\in \mathcal{J}} \frac{1}{\sum_{i\in \mathcal{I}}p^i_j}p^i_j q_j \quad \forall i\in \mathcal{I} \\
& && y_{ij} = -y_{ji} \quad \forall i,j \in \mathcal{I} \\
& && x_{ij} \in \{0,1\} \quad \forall i,j \in \mathcal{I},
\end{align*}
$$

and *voila*!



# The code

As always, I have coded the problem in Python. I am using the libraries `cvxpy` and `cvxopt` , the latter only provides the solver needed for integer problems. The data for the problem is written in a separate file named `data.txt` and looks like this

```
Sara, Miguel, Rubén, Adrian, Patri, Mario
Patri paid 863 for the expense Lechuga for the people Miguel, Patri, Sara
Rubén paid 250 for the expense Ketchup for the people Adrian, Patri, Rubén, Sara, Miguel, Mario
Adrian paid 100 for the expense Mayonesa for the people Adrian, Patri, Rubén, Sara
Adrian paid 200 for the expense Tomate for the people Adrian, Sara, Patri, Miguel
Miguel paid 400 for the expense Pan de hamburguesa for the people Adrian, Patri, Rubén, Sara, Miguel, Mario
Sara paid 422 for the expense Burgers for the people Adrian, Miguel, Patri, Rubén, Sara, Mario
```

The functions used for importing the data into the program are

```python
def import_data(file):
    with open(file, 'r', encoding='utf8') as f:
        people = f.readline().strip('\n').split(', ')
        matrix = [list() for p in people]
        m = [0 for p in people]
        expenses = list()
        q = list()
        for line in f:
            parse_line(line.strip('\n'), people, expenses, m, q, matrix)
    return people, expenses, m, q, expenses, np.matrix(matrix)
```

```python
def parse_line(line, people, expenses, m, q, matrix):
    # get variables from line
    payer, rest = line.split(' paid ')
    money, rest = rest.split(' for the expense ')
    expense, debtors = rest.split(' for the people ')
    # fill in the data
    m[people.index(payer)] += float(money)
    expenses.append(expense)
    q.append(float(money))
    debtors = debtors.split(', ')
    for p in people:
        matrix[people.index(p)].append(1 if p in debtors else 0)
```



And the code for solving the problem is the following. You can find the documentation for `cvxpy` in their [website](https://www.cvxpy.org/tutorial/intro/index.html).

```python
# GET THE DATA
people, expenses, m, q, expenses, P = import_data('data.txt')
l, n = len(expenses), len(people)

# FORMULATE THE PROBLEM
b = np.array([ sum([1/sum(P[:,j]) *P[i,j]*q[j] for j in range(l)]) - m[i] for i in range(n)]).squeeze()
x = cp.Variable((n,n), boolean = True)
y = cp.Variable((n,n))
const1 = [y[i,j] <= sum(m)*x[i,j] for i in range(n) for j in range(n)]
const2 = [ (sum( [y[i,j] for j in range(n)] ) == b[i]) for i in range(n)]
const3 = [y[i,j] == -y[j,i] for i in range(n) for j in range(n)]
objective = cp.Minimize( sum( [x[i,j] for i in range(n) for j in range(n)]))

# SOLVE IT
prob = cp.Problem(objective, const1 + const2 + const3)
prob.solve()

# FORMAT OUTPUT
solution = ['{} has to pay {} {}'.format(people[i], people[j], y.value[i,j]) for i in range(n) for j in range(n) if x.value[i,j]]

print("Optimal number of transactions:", prob.value)
for r in solution:
    print(r)
```



[^1]:In a previous version of the post it was incorrectly assumed that all $y_{ij}$ had to be non-negative (and the condition $y_{ij} = -y_{ji}$ was omitted). This, together with constraint $(1)$ made the problem infeasible if there were people who had paid more than their fair share $m_i > \frac{1}{n}M$. The same with was true for the constraint $(2)$.

