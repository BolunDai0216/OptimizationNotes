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

```{admonition} ADMM
:class: tip

- $x^{(k)} = \underset{x}{\mathrm{argmin}}\ L_\rho(x, z^{(k-1)}, u^{(k-1)})$
- $z^{(k)} = \underset{z}{\mathrm{argmin}}\ L_\rho(x^{(k)}, z, u^{(k-1)})$
- $u^{(k)} = u^{(k-1)} + \rho(Ax^{(k)} + Bz^{(k)} - c)$
```

The ADMM algorithm is also expressed in the **scaled form**, where we define $w = u / \rho$, and write the above steps as

```{admonition} ADMM in Scaled Form
:class: tip

- $x^{(k)} = \displaystyle \underset{x}{\mathrm{argmin}}\ f(x) + \frac{\rho}{2}\Big\|Ax + Bz^{(k-1)} - c + w^{(k-1)}\Big\|_2^2$
- $z^{(k)} = \displaystyle \underset{x}{\mathrm{argmin}}\ g(z) + \frac{\rho}{2}\Big\|Ax^{(k)} + Bz - c + w^{(k-1)}\Big\|_2^2$
- $w^{(k)} = w^{(k-1)} + Ax^{(k)} + Bz^{(k)} - c$
```
## Connection to Proximal Operators

If we consider the problem of

$$
\min_x f(x) + g(x)
$$

we can write it in ``ADMM" form

$$
\begin{align}
    \min_{x, z}\ &\ f(x) + g(z)\\
    \mathrm{subject\ to}\ &\ x = z
\end{align}
$$

We can then write the ADMM updates in scaled form. Note that in this case we have $A = I$, $B = -I$, and $c = \mathbf{0}$. First, we have the $x$ update as

$$
\begin{align}
    x^+ &= \underset{x}{\mathrm{argmin}}\Big(f(x) + \frac{\rho}{2}\Big\|x - z + w\Big\|_2^2\Big)\\
        &= \underset{x}{\mathrm{argmin}}\Big(\frac{1}{2\cdot 1/\rho}\Big\|x - z + w\Big\|_2^2 + f(x)\Big)\\
        &= \mathrm{prox}_{f, 1/\rho}(z - w).
\end{align}
$$

Then, we have the $z$ update as

$$
\begin{align}
    z^+ &= \underset{z}{\mathrm{argmin}}\Big(g(z) + \frac{\rho}{2}\Big\|x^+ - z + w\Big\|_2^2\Big)\\
        &= \underset{z}{\mathrm{argmin}}\Big(\frac{1}{2\cdot 1/\rho}\Big\|z - (x^+ + w)\Big\|_2^2 + g(z)\Big)\\
        &= \mathrm{prox}_{g, 1/\rho}(x^+ + w).
\end{align}
$$

Finally, we have the $w$ update as

$$
w^+ = w + x^+ - z^+.
$$
