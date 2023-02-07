# Augmented Lagrangian

One issue with the dual gradient ascent approach is that the convergence guatantees are rather strong (requires strong convexity). To deal with this issue, we augment the objective function $f(x)$,

$$
\begin{align}
    \min_x\ &\ f(x) + \frac{\rho}{2}\|Ax - b\|_2^2\\
    \mathrm{subject\ to}\ &\ Ax = b.
\end{align}
$$
