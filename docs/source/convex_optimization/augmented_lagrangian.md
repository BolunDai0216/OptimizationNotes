# Augmented Lagrangian

One issue with the dual gradient ascent approach is that the convergence guatantees are rather strong (requires strong convexity). To deal with this issue, we augment the objective function $f(x)$,

$$
\begin{align}
    \min_x\ &\ f(x) + \frac{\rho}{2}\|Ax - b\|_2^2\\
    \mathrm{subject\ to}\ &\ Ax = b
\end{align}
$$

with $\rho > 0$. When the constraint is satisfied, we can see the augmented objective function should have the same value as the original objective function. Additionally, we can see that the objective function is now also strong convex, assuming $A$ has full column rank. Then, we have the augmented Lagrangian (AL) method for equality constraints as 

```{admonition} AL with Equality Constraints
:class: tip

Start for an initial dual guess $u^{(0)}$, and repeats for $k = 1, 2, 3, \cdots$

- $\displaystyle x^{(k)} \in \underset{x}{\mathrm{argmin}}\Big[f(x) + \frac{\rho}{2}\|Ax - b\|_2^2 + (u^{(k-1)})^TAx\Big]$
- $u^{(k)} = u^{(k-1)} + \rho(Ax^{(k)} - b)$

where the step sizes are chosen to be $t_k = \rho$.
```

We can see that because of the quadratic term AL loses the decomposibility, but in return it has much better convergence properties. When we have inequality constraints, the AL formulation becomes

$$
\begin{align}
    \min_x\ &\ f(x) + \frac{\rho}{2}\|\min\{0, b - Ax\}\|_2^2\\
    \mathrm{subject\ to}\ &\ Ax \leq b
\end{align}
$$

```{admonition} AL with Inequality Constraints
:class: tip

Start for an initial dual guess $v^{(0)}$, and repeats for $k = 1, 2, 3, \cdots$

- $\displaystyle x^{(k)} \in \underset{x}{\mathrm{argmin}}\Big[f(x) + \frac{\rho}{2}\|\min\{0, b - Ax\}\|_2^2 + (v^{(k-1)})^TAx\Big]$
- $v^{(k)} = \Big[v^{(k-1)} + \rho(Ax^{(k)} - b)\Big]_+$
- $\rho \leftarrow \alpha\rho$.
```

When there are both equality $Ax = b$ and inequality $Cx \leq d$ constraints, define

$$
\tilde{f}(x) = f(x) + \frac{\rho_e}{2}\|Ax - b\|_2^2 + \frac{\rho_i}{2}\|\min\{0, d - Cx\}\|_2^2
$$

we have the general form of AL as

```{admonition} AL with Inequality Constraints
:class: tip

Start for an initial dual guess $v^{(0)}$, and repeats for $k = 1, 2, 3, \cdots$

- $\displaystyle x^{(k)} \in \underset{x}{\mathrm{argmin}}\Big[\tilde{f}(x) + (u^{(k-1)})^TAx + (v^{(k-1)})^TAx\Big]$
- $u^{(k)} = u^{(k-1)} + \rho_e(Ax^{(k)} - b)$
- $v^{(k)} = \Big[v^{(k-1)} + \rho_i(Cx^{(k)} - d)\Big]_+$
- $\rho_i \leftarrow \alpha\rho_i$
```

