# Primal Dual Interior Point Method

The primal dual interior point method (PD-IPOpt) is one of the workhorses of modern optimization software. PD-IPOpt solves the same kind of problems as barrier methods

$$
\begin{align}
\min_x\ &\ f(x)\\
\mathrm{subject\ to}\ &\ h_i(x) \leq 0,\ i = 1, \cdots, m\\
                      &\ Ax = b. 
\end{align}
$$(eqn:pd_ipopt_problem)

Starting from the pertubed KKT conditions

```{admonition} Perturbed KKT Conditions
:class: tip 
- Stationarity: $\displaystyle\nabla f(x^\star(t)) + \sum_{i=1}^{m}{v_i^\star(t)\nabla h_i(x^\star(t))} + A^Tu^\star(t) = 0$;
- Primal feasibility: $Ax^\star(t) = b$ and $h_i(x^\star(t)) \leq 0$;
- Dual feasibility: $v_i^\star(t) > 0$;
- Complementary slackness: $\displaystyle v_i^\star(t)h_i(x^\star(t)) = -1/t$.
```

If extract out the equations in the pertubed KKT conditions, we have

$$
r(x, u, v) = \begin{bmatrix}
\nabla f(x) + \mathrm{diag}(v)\nabla h(x) + A^Tu\\
-\mathrm{diag}(v)h(x) - 1/t\\
Ax - b
\end{bmatrix} = 0.
$$(eqn:dp_ipopt_kkt_cond)

Then, we can form a root finding problem on {eq}`eqn:dp_ipopt_kkt_cond`, and the solution to this root finding problem gives us both the primal and dual optimal variables.

## Algorithm

First, we define the terms

$$
\begin{align}
r_\mathrm{dual} &= \nabla f(x) + \mathrm{diag}(v)\nabla h(x) + A^Tu\\
r_\mathrm{cent} &= -\mathrm{diag}(v)h(x) - 1/t\\
r_\mathrm{prim} &= Ax - b.
\end{align}
$$(eqn:dp_ipopt_components)

Then, at each time step with the current primal and dual varaible iterates $(x, u, v)$, we perform a Newton step on $r(x, u, v)$

$$
0 = r(x, u, v) + \nabla r(x, u, v)\begin{bmatrix}
    \Delta{x}\\
    \Delta{u}\\
    \Delta{v}
\end{bmatrix}
$$

with 

$$
\nabla r(x, u, v) = \begin{bmatrix}
    \nabla^2 f(x) + \mathrm{diag}(v)\nabla^2 h(x) & A & \nabla h(x)\\
    -\mathrm{diag}(v)\nabla h(x) & 0 & -h(x)\\
    A & 0 & 0\\
\end{bmatrix}.
$$

This update is the core of the PD-IPOpt method.



## Backtracking Line Search

The backtracing line search is a variant of the version for damped Newton's method.
