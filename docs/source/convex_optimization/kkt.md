# Kurush-Kuhn-Tucker (KKT) Conditions

This page will discuss the famous KKT conditions. The KKT conditions consists of four components:

```{admonition} Stationarity
:class: tip

Zero belongs to the subderivatives of the Lagrangian.

$$
0 \in \partial_x\Big(f(x) + \sum_{i=1}^{m}{u_i\ell_i(x)} + \sum_{j=1}^{r}{v_jh_j(x)}\Big)
$$
```

```{admonition} Primal Feasibility
:class: tip

The constraints of the primal problem should be satisfied

$$
\begin{align}
\ell_i(x) &= 0 & i = 1, \cdots, m\\
h_j(x) &\leq 0 & j = 1, \cdots, r.
\end{align}
$$
```

```{admonition} Dual Feasibility
:class: tip

The constraints of the dual problem should be satisfied

$$
v_j \geq 0 \quad j = 1, \cdots, r.
$$
```

```{admonition} Complementary Slackness
:class: tip

For non-active constraints the corresponding dual variable is zero.

$$
\sum_{j=1}^{r}{v_jh_j(x)} = 0.
$$
```

**The KKT conditions provides a sufficient condition for optimality**. If a set of primal and dual variables $\{x^\star, u^\star, v^\star\}$ satsifies the KKT condition then they are the primal and dual solutions to the optimization problem.

**If strong duality holds**, then for the primal and dual solutions $\{x^\star, u^\star, v^\star\}$ they satisfy the KKT conditions. **Under strong duality, the KKT conditions provides a necessary condition for optimality**.