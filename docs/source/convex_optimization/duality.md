# Duality

Duality is an important concept in optimization. We can view it from two angles. First, we use linear programs (LPs) to illustrate the point. Then we extend the notion of duality to general problems.

## First Perspective Using LPs

If we have a linear program in the form of 

$$
\begin{align}
    \min_{x, y}\ &\ px + qy\\
    \mathrm{subject\ to}\ &\ x + y \geq 2\\
                          &\ x, y \geq 0
\end{align}
$$(eqn:primal_LP)

First, we multiply each of the inequality constraints with a coefficient

$$
\begin{align}
    a(x + y) &\geq 2a\\
    bx &\geq 0\\
    cy &\geq 0\\
    a, b, c &\geq 0.
\end{align}
$$(eqn:primal_LP_constraints_multiplied)

Then, we add the above three inequalities, to get

$$
(a+b)x + (a+c)y \geq 2a.
$$(eqn:primal_LP_constraints_multiplied_added)

If the conditions $a + b = p$ and $a + c = q$ holds, then the solution to the original problem {eq}`eqn:primal_LP` would be $2a$. However, there may be many sets of $\{a, b, c\}$'s that satisfy

$$
\begin{align}
    a + b &= p\\
    a + c &= q\\
    a, b, c &\geq 0.
\end{align}
$$(eqn:dual_conditions_LP)

So we would want to pick the set of $\{a, b, c\}$ that gives us the largest value of $2a$. We can write this as another optimizal problem

$$
\begin{align}
    \max_{a, b, c}\ &\ 2a\\
    \mathrm{subject\ to}\ &\ a + b = p\\
                          &\ a + c = q\\
                          &\ a, b, c \geq 0.
\end{align}
$$(eqn:dual_LP)

And the solution to {eq}`eqn:dual_LP` would be the same as the solution to {eq}`eqn:primal_LP`. We see that {eq}`eqn:primal_LP` is a minimization problem and {eq}`eqn:dual_LP` is a maximization problem. Thus, there exists a dual relationship between these two problems. By convention, we would call {eq}`eqn:primal_LP` the primal problem and {eq}`eqn:dual_LP` the dual problem. Similarly, we call $x, y$ the primal variables and $a, b, c$ the dual variables.

If there exists an equality constraint

$$
\begin{align}
    \max_{x, y}\ &\ px + qy\\
    \mathrm{subject\ to}\ &\ x \geq 0\\
                          &\ y \leq 1\\
                          &\ 3x + y = 2
\end{align}
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
\begin{align}
    \max_{a, b, c}\ &\ 2c - b\\
    \mathrm{subject\ to}\ &\ 3c + a = p\\
                          &\ c - b = q\\
                          &\ a, b \geq 0.
\end{align}
$$(eqn:dual_LP_eq)

### General form of duality in LPs

For a general LP in the form of

$$
\begin{align}
    \min_{x}\ &\ c^Tx\\
    \mathrm{subject\ to}\ &\ Ax = b\\
                          &\ Gx \leq h
\end{align}
$$(eqn:general_primal_LP)

we define two vectors $u\in\mathbb{R}^{n_e}$ and $v\in\mathbb{R}_+^{n_i}$, where $n_e$ is the number of equality constraints and $n_i$ is the number of inequality constraints. We perform the same steps

- multiply the coefficients with the constraints

$$
\begin{align}
    u^T(Ax - b) &= 0\\
    -v^T(Gx - h) &\geq 0\\
    v &\geq 0.
\end{align}
$$(eqn:general_primal_LP_step_1)

- sum up the constraints

$$
u^T(Ax - b) + v^T(Gx - h) \leq 0 \rightarrow \underbrace{-(A^Tu + G^Tv)^T}_{:=c^T}x \geq -b^Tu - h^Tv
$$(eqn:general_primal_LP_step_2)

- form the dual LP

$$
\begin{align}
    \min_{u, v}\ &\ -b^Tu - h^Tv\\
    \mathrm{subject\ to}\ &\ -(A^Tu + G^Tv) = c\\
                          &\ v \geq 0.
\end{align}
$$(eqn:general_dual_LP)

## Second Perspective Using LPs

For the second perspective we directly consider the general LP 

\begin{align*}
    \min_{x}\ &\ c^Tx\\
        \mathrm{subject\ to}\ &\ Ax = b\\
                            &\ Gx \leq h
\end{align*}

and we can see that for any $x$ that is feasible the following relationship holds

$$
c^T + \underbrace{u^T\underbrace{(Ax-b)}_{= 0} + v^T\underbrace{(Gx-h)}_{\leq 0}}_{\leq 0} \leq c^Tx.
$$(eqn:lagrangian_preview)

We define the set of all feasible $x$ as $\mathbb{C}$

$$
\mathbb{C} = \{x \mid Ax-b = 0, Gx-h \leq 0\}.
$$

Then, we have the following relationship

$$
\begin{align}
    f^\star &= \min_{x\in\mathbb{C}}\ c^Tx\\
            &\geq \min_{x\in\mathbb{C}}\ c^Tx + u^T(Ax-b) + v^T(Gx-h)\\
            &\geq \underbrace{\min_{x}\ c^Tx + u^T(Ax-b) + v^T(Gx-h)}_{:=g(u, v)}
\end{align}
$$(eqn:lagrangian_derivation)

i.e. by construction $g(u, v) \leq f^\star$ when $v \geq 0$. 

If we write $g(u, v)$ for our general LP in {eq}`eqn:general_primal_LP`, it is

$$
\begin{align}
    g(u, v) &= \min_{x}\ c^Tx + u^T(Ax-b) + v^T(Gx-h)\\
            &= \min_{x}\ (c + A^Tu + G^Tv)^Tx - b^Tu - h^Tv\\
            &= \begin{cases}
                -\infty & \text{if}\ c + A^Tu + G^Tv \neq 0\\
                -b^Tu - h^Tv & \text{if}\ c + A^Tu + G^Tv = 0
            \end{cases}.
\end{align}
$$

Then, we can get an equivalence between the optimization problem that maximizes $g(u, v)$ and the dual LP:

$$
\begin{align}
    &\quad\quad\ \max_{u, v}\ g(u, v) & \Longleftrightarrow &\quad\quad\quad\quad\ \max_{u, v}\ -b^Tu - h^Tv\\
    &\mathrm{subject\ to}\ \ v \geq 0 & &\quad\quad\ \mathrm{subject\ to}\ \ c + A^Tu + G^Tv = 0\\
    & & & \quad\quad\quad\quad\quad\quad\quad v \geq 0
\end{align}
$$

This perspective gives us another way of arriving at the dual problem, which is more general than the first perspective and can be easily extended to general optimization problems other than just LPs.

## Duality in General Optimization Problems

The general form of minization problems can be written as

$$
\begin{align}
    \min_{x}\ &\ f(x)\\
    \mathrm{subject\ to}\ &\ \ell_i(x) = 0 & i=1, \cdots, m\\
                          &\ h_j(x) \leq 0 & j=1, \cdots, r.
\end{align}
$$(eqn:general_min_opt)

```{admonition} Definition
:class: tip
The Lagrangian is defined as

$$
\mathcal{L}(x, u, v) = f(x) + \sum_{i=1}^{m}{u_i\ell_i(x)} + \sum_{j=1}^{r}{v_jh_j(x)}
$$(eqn:lagrangian_def)

with $u\in\mathbb{R}^m$ and $v\in\mathbb{R}_+^r$.
```
Following the derivation in the above section, we can see that for each feasible $x$ the following relationship holds:

$$
\mathcal{L}(x, u, v) = f(x) + \underbrace{\underbrace{\sum_{i=1}^{m}{u_i\ell_i(x)}}_{= 0} + \underbrace{\sum_{j=1}^{r}{v_jh_j(x)}}_{\leq 0}}_{\leq 0} \leq f(x).
$$

If we denote the primal feasible set as $\mathbb{C} = \{x \mid \ell(x) = 0, h(x) \leq 0\}$ and $f^\star$ the primal optimal value, then we have the relationship

$$
\begin{align}
    f^\star &= \min_{x\in\mathbb{C}}\ f(x)\\
            &\geq \min_{x\in\mathbb{C}}\ \mathcal{L}(x, u, v)\\
            &\geq \min_{x}\ \mathcal{L}(x, u, v)\\
            &:= g(u, v).
\end{align}
$$

```{admonition} Definition
:class: tip
The Lagrangian dual function is defined as

$$
g(u, v) = \min_{x}\mathcal{L}(x, u, v)
$$(eqn:lagrangian_dual_function_def)
```

The Lagrangian dual function provides a lower bound to the solution to the primal problem. We can also see that the Lagrangian dual function is always concave (with respect to $u$ and $v$)

$$
\begin{align}
    g(u, v) &= \min_{x}\Big\{f(x) + \sum_{i=1}^{m}{u_i\ell_i(x)} + \sum_{j=1}^{r}{v_jh_j(x)}\Big\}\\
            &= -\max_{x}\underbrace{\Big\{-f(x) - \sum_{i=1}^{m}{u_i\ell_i(x)} - \sum_{j=1}^{r}{v_jh_j(x)}\Big\}}_{\text{pointwise maximum of convex functions in $u$ and $v$}}.
\end{align}
$$(eqn:lagrangian_dual_function_concave)

The Lagrangian dual problem is about finding the best (largest) lower bound.

```{admonition} Definition
:class: tip
The Lagrangian dual problem is defined as

$$
\begin{align}
\max_{u, v}\ &\ g(u, v)\\
\mathrm{subject\ to}\ &\ v\geq0.
\end{align}
$$(eqn:lagrangian_dual_problem_def)
```

The Lagrangian dual problem is always a concave maximization problem which is the same as a convex minimization problem.

## Duality Gaps

One question you might have is now that we have the Lagrangian dual problem, if we solve it will it give us the same result as solving the primal problem. If it does we see that there is strong duality, i.e. $f^\star = g^\star$, otherwise $f^\star \geq g^\star$, which we call weak duality. Since the Lagrangian dual function provides a lower bound to the solution to the primal problem, $g^\star$ will never be larger than $f^\star$. 

```{admonition} Slater's Condition
:class: tip

If the primal is a convex problem, and ther exists at least one strictly feasible $x\in\mathbb{R}^n$, i.e. $\ell_i(x) = 0$ and $h_j(x) < 0$,  then strong duality holds. If some of the $h_j$ are affine, then only $h_j(x) \leq 0$ needs to be satisfied.
```
