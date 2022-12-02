# Convex Optimization

The general form of convex optimization problems is

$$
\begin{eqnarray}
    \min_{x\in\mathbb{R}^n}\ &\ f(x)\\
    \mathrm{subject\ to}\ &\ c(x) \leq 0\\
                          &\ d(x) = 0
\end{eqnarray}
$$(eqn:convex_opt_general_form)

where $f(x)$ and $c(x)$ are convex functions, and $d(x)$ is an affine function. The reason $d(x)$ is required to be affine but not convex, is that we can rewrite the constraint $d(x) = 0$ as

$$
\begin{eqnarray}
    d(x) &\leq 0\\
    -d(x) &\leq 0\\
\end{eqnarray}
$$

and the only kind of function that makes both $d(x)$ and $-d(x)$ convex are affine functions.


```{toctree}
:hidden:

duality
```