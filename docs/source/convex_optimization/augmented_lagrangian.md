# Augmented Lagrangian

One issue with the dual gradient ascent approach is that the convergence guatantees are rather strong (requires strong convexity). To deal with this issue, we augment the objective function $f(x)$,

$$
\begin{align}
    \min_x\ &\ f(x) + \frac{\rho}{2}\|Ax - b\|_2^2\\
    \mathrm{subject\ to}\ &\ Ax = b
\end{align}
$$

with $\rho > 0$. When the constraint is satisfied, we can see the augmented objective function should have the same value as the original objective function. Additionally, we can see that the objective function is now also strong convex, assuming $A$ has full column rank. Then, we have the augmented Lagrangian method for equality constraints as 

```{admonition} Dual Decomposition Method with Inequality Constraints
:class: tip

Start for an initial dual guess $u^{(0)}$, and repeats for $k = 1, 2, 3, \cdots$

- $\displaystyle x^{(k)} \in \underset{x}{\mathrm{argmin}}\Big[f(x) + \frac{\rho}{2}\|Ax - b\|_2^2 + (u^{(k-1)})^TAx\Big]$
- $u^{(k)} = u^{(k-1)} + \rho(Ax^{(k)} - b)$

where the step sizes $t_k$ are chosen using backtracing line search and $(u_+)_i = \max\{0, u_i\}.$
```


