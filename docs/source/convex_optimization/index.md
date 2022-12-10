# Convex Optimization

This section of the notes will discuss convex optimization. In this page the basic concepts will be presented and the remaining topic will be provided in individual pages.

## Basics

Convex functions are defined as any function $f: \mathbb{R}^n \rightarrow \mathbb{R}$ that satisfies

$$
\begin{eqnarray}
   f(tx + (1-t)y) \leq tf(x) + (1-t)f(y),\quad\forall t\in[0, 1].
\end{eqnarray}
$$(eqn:convex_func)

Intuitively, we can see this as if we draw a straight line between two points on $f(x)$, then the line should lie above $f(x)$.

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
$$(eqn:d_x_affine_explanation)

and the only kind of function that makes both $d(x)$ and $-d(x)$ convex are affine functions.


```{toctree}
:hidden:

duality
kkt
newtons_method
barrier_method
primal_dual_ipopt
```