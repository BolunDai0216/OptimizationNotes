# Alternating Direction Method of Multipliers

Consider the problem 

$$
\begin{align}
\min_{x, z}\ &\ f(x) + g(z)\\
\mathrm{subject\ to}\ &\ Ax + Bz = c
\end{align}
$$

We then augment the objective with a strongly convex term

$$
\begin{align}
\min_{x, z}\ &\ f(x) + g(z) + \frac{\rho}{2}\Big\|Ax + Bz - c\Big\|_2^2\\
\mathrm{subject\ to}\ &\ Ax + Bz = c
\end{align}
$$

with $\rho > 0$. We then have the augemented Lagrangian as

$$
L_\rho(x, z, u) = f(x) + g(z) + u^T(Ax + Bz - c) + \frac{\rho}{2}\Big\|Ax + Bz - c\Big\|_2^2.
$$

The ADMM algorithm then repeats the following steps

- $x^{(k)} = \underset{x}{\mathrm{argmin}}\ L_\rho(x, z^{(k-1)}, u^{(k-1)})$
- $z^{(k)} = \underset{z}{\mathrm{argmin}}\ L_\rho(x^{(k)}, z, u^{(k-1)})$
- $u^{(k)} = u^{(k-1)} + \rho(Ax^{(k)} + Bz^{(k)} - c)$

The ADMM algorithm is also expressed in the **scaled form**, where we define $w = u / \rho$, and write the above steps as

- $x^{(k)} = \displaystyle \underset{x}{\mathrm{argmin}}\ f(x) + \frac{\rho}{2}\Big\|Ax + Bz^{(k-1)} - c + w^{(k-1)}\Big\|_2^2$
- $z^{(k)} = \displaystyle \underset{x}{\mathrm{argmin}}\ g(z) + \frac{\rho}{2}\Big\|Ax^{(k)} + Bz - c + w^{(k-1)}\Big\|_2^2$
- $w^{(k)} = w^{(k-1)} + Ax^{(k)} + Bz^{(k)} - c$
