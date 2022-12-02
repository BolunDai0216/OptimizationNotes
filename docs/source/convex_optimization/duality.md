# Duality

Duality is an important concept in optimization. We can view it from two angles. First, we use linear programs (LPs) to illustrate the point. Then we extend the notion of duality to general problems.

## First Perspective Using LPs

If we have a linear program in the form of 

$$
\begin{eqnarray}
    \min_{x, y}\ &\ px + qy\\
    \mathrm{subject\ to}\ &\ x + y \geq 2\\
                          &\ x, y \geq 0
\end{eqnarray}
$$(eqn:primal_LP)

First, we multiply each of the inequality constraints with a coefficient

$$
\begin{eqnarray}
    a(x + y) &\geq 2a\\
    bx &\geq 0\\
    cy &\geq 0\\
    a, b, c &\geq 0.
\end{eqnarray}
$$(eqn:primal_LP_constraints_multiplied)

Then, we add the above three inequalities, to get

$$
(a+b)x + (a+c)y \geq 2a.
$$(eqn:primal_LP_constraints_multiplied_added)

If the conditions $a + b = p$ and $a + c = q$ holds, then the solution to the original problem {eq}`eqn:primal_LP` would be $2a$. However, there may be many sets of $\{a, b, c\}$'s that satisfy

$$
\begin{eqnarray}
    a + b &= p\\
    a + c &= q\\
    a, b, c &\geq 0.
\end{eqnarray}
$$(eqn:dual_conditions_LP)

So we would want to pick the set of $\{a, b, c\}$ that gives us the largest value of $2a$. We can write this as another optimizal problem

$$
\begin{eqnarray}
    \max_{a, b, c}\ &\ 2a\\
    \mathrm{subject\ to}\ &\ a + b = p\\
                          &\ a + c = q\\
                          &\ a, b, c \geq 0.
\end{eqnarray}
$$(eqn:dual_LP)

And the solution to {eq}`eqn:dual_LP` would be the same as the solution to {eq}`eqn:primal_LP`. We see that {eq}`eqn:primal_LP` is a minimization problem and {eq}`eqn:dual_LP` is a maximization problem. Thus, there exists a dual relationship between these two problems. By convention, we would call {eq}`eqn:primal_LP` the primal problem and {eq}`eqn:dual_LP` the dual problem. Similarly, we call $x, y$ the primal variables and $a, b, c$ the dual variables.

If there exists an equality constraint

$$
\begin{eqnarray}
    \max_{x, y}\ &\ px + qy\\
    \mathrm{subject\ to}\ &\ x \geq 0\\
                          &\ y \leq 1\\
                          &\ 3x + y = 2
\end{eqnarray}
$$(eqn:primal_LP_eq)

we also multiply each of the constraints with non-negative coefficients $a, b$ and $c \in \mathbb{R}$, and sum them up

$$
\begin{cases}
    ax \geq 0\\
    -by \geq -b\\
    3cx + cy = 2c\\
    a, b \geq 0
\end{cases} \quad \xrightarrow{\mathrm{Summation}} \quad \underbrace{(3c + a)}_{:=p}x + \underbrace{(c - b)}_{:=q}y \geq 2c - b.
$$(eqn:primal_LP_eq_constraints_multiplied_added)

```{admonition} Note
:class: note
Here $c$ is unconstrained!
```

Then, we can get the corresponding dual problem as

$$
\begin{eqnarray}
    \max_{a, b, c}\ &\ 2c - b\\
    \mathrm{subject\ to}\ &\ 3c + a = p\\
                          &\ c - b = q\\
                          &\ a, b \geq 0.
\end{eqnarray}
$$(eqn:dual_LP_eq)

### General form of duality in LPs

For a general LP in the form of

$$
\begin{eqnarray}
    \min_{x}\ &\ c^Tx\\
    \mathrm{subject\ to}\ &\ Ax = b\\
                          &\ Gx \leq h
\end{eqnarray}
$$(eqn:general_primal_LP)

we define two vectors $u\in\mathbb{R}^{n_e}$ and $v\in\mathbb{R}_+^{n_i}$, where $n_e$ is the number of equality constraints and $n_i$ is the number of inequality constraints. We perform the same steps

- multiply the coefficients with the constraints

$$
\begin{eqnarray}
    u^T(Ax - b) &= 0\\
    -v^T(Gx - h) &\geq 0\\
    v &\geq 0.
\end{eqnarray}
$$(eqn:general_primal_LP_step_1)

- sum up the constraints

$$
u^T(Ax - b) + v^T(Gx - h) \leq 0 \rightarrow \underbrace{-(A^Tu + G^Tv)^T}_{:=c^T}x \geq -b^Tu - h^Tv
$$(eqn:general_primal_LP_step_2)

- form the dual LP

$$
\begin{eqnarray}
    \min_{u, v}\ &\ -b^Tu - h^Tv\\
    \mathrm{subject\ to}\ &\ -(A^Tu + G^Tv) = c\\
                          &\ v \geq 0.
\end{eqnarray}
$$(eqn:general_dual_LP)