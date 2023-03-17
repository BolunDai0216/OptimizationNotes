# Subgradients

For a convex and differentiable function $f$, the gradient of $f$, denoted as $\nabla f$, satisfies

$$
f(y) \geq f(x) + \nabla f(x)^T(y - x),\ \forall x, y.
$$

For functions that are convex but not necessarily differentiable (e.g. $f(x) = |x|$), we can generalize the concept of gradients to subgradients. The subgradient $g\in\mathbb{R}^n$ of a convex function $f$ at $x$ satisfies

$$
f(y) \geq f(x) + g^T(y - x),\ \forall y.
$$(eqn:subgradient_def)


```{admonition} Subgradients Properties
:class: tip
- The subgradient always exists within the relative interior of the domain. It does not necessarily exist at the boundary, e.g., when there is an indicator functions.
- When $f$ is differentiable at $x$, then $g = \nabla f(x)$ uniquely.
```

## Subdifferential

The set of all subgradients of a convex function $f$ is called the subdifferential

$$
\partial f(x) = \{g \mid g\in\mathbb{R}^n, g\ \text{is a subgradient of}\ f\ \text{at}\ x\}.
$$

```{admonition} Subdifferential Properties
:class: tip
- $\partial f(x)$ is always closed and convex.
- $\partial f(x)$ is nonempty for convex $f$s.
- If $f$ is differentiable at $x$, then $\partial f(x) = \{\nabla f(x)\}$.
- If $\partial f(x) = \{g\}$, then $f$ is differentiable at $x$ and $\nabla f(x) = g$.
```

### Subgradient Calculus

- Scaling: $\partial(af) = a\partial(f)$ when $a > 0$.
- Addition: $\partial(f_1 + f_2) = \partial f_1 + \partial f_2$.
- Affine composition: If $h(x) = f(Ax + b)$ then $\partial h(x) = A^T\partial f(Ax + b)$.
- Finite pointwise maximum: if $f(x) = \max_{i = 1, \cdots, m}f_i(x)$, then the subdifferential is the convex hull of the union of the subdifferentials of all active functions $f_i$, where the maximum is obtained, i.e., $f_i(x) = f(x)$, 

$$\partial f(x) = \mathrm{conv}\Big(\bigcup_{i: f_i(x) = f(x)}\partial f_i(x)\Big)$$

- General pointwise maximum: if $f(x) = \max_{s \in S}f_s(x)$, then

$$\partial f(x) = \mathrm{cl}\Big\{\mathrm{conv}\Big(\bigcup_{s: f_s(x) = f(x)}\partial f_s(x)\Big)\Big\}$$


### Subdifferential of an Indicator Function

For a convex set $C \subseteq \mathbb{R}^n$, the indicator function on set $C$ is defined as $I_C: \mathbb{R}^n \rightarrow \mathbb{R}^n$:

$$
I_C(x) = \begin{cases}
    0 & \mathrm{if}\ x \in C\\
    \infty & \mathrm{if}\ x \notin C
\end{cases}
$$

The normal cone $\mathcal{N}_C(x)$ of the set $C$ at $x$ is defined as

$$
\mathcal{N}_C(x) = \{g \mid g\in\mathbb{R}^n, g^Tx \geq g^Ty\ \text{for any}\ y \in C\}.
$$

Then, we have the relationship

$$
\partial I_C(x) = \mathcal{N}_C(x).
$$

```{dropdown} Proof
From {eq}`eqn:subgradient_def`, we know that the subgradient of $I_C$ satisfies the relationship

$$
I_C(y) \geq I_C(x) + g^T(y - x), \forall y \in C.
$$

Since $x, y \in C$, we have $I_C(x) = I_C(y) = 0$. Thus, the above inequality becomes

$$
0 \geq 0 + g^T(y - x) \rightarrow g^Tx \geq g^Ty.
$$

Since any $g$ that satisfies $g^Tx \geq g^Ty$ will belong to $\mathcal{N}_C(x)$, we can conclude that $\partial I_C(x) = \mathcal{N}_C(x)$.
```

### Subgradient Optimality Condition

For any $f$ (can be nonconvex), we have 

$$
f(x^\star) = \min_x f(x) \iff 0 \in \partial f(x^\star)
$$

In other words, $x^\star$ is a minimizer if and only if $0$ belongs to the subgradient of $f$ at $x^\star$. We can easily understand this by setting $g = 0$ in {eq}`eqn:subgradient_def`, which leads to

$$
f(y) \geq f(x^\star) + 0^T(y - x^\star) = f(x^\star).
$$

Using this, we can also derive a variant of the first-order optimality condition

$$
0 \in \partial f(x) + \mathcal{N}_C(x).
$$

## Examples

### Lasso Optimality Conditions

Given $y \in \mathbb{R}^n$, $X \in \mathbb{R}^{n \times p}$, and $\lambda \geq 0$, the Lasso problem can be written as

$$
\min_\beta \frac{1}{2}\|y - X\beta\|_2^2 + \lambda\|\beta\|_1.
$$

Using the subgradient optimality condition, we have

$$
0 \in \partial\Big(\frac{1}{2}\|y - X\beta\|_2^2 + \lambda\|\beta\|_1\Big) = -X^T(y - X\beta) + \lambda\partial\|\beta\|_1.
$$

Which is equivalent to saying 

$$
X^T(y - X\beta) = \lambda v
$$

with $v \in \partial\|\beta\|_1$. We can write our the individual elements of $v$ as

$$
v_i = \begin{cases}
    \{1\} & \mathrm{if}\ \beta_i > 0\\
    \{-1\} & \mathrm{if}\ \beta_i < 0\\
    [-1, 1] & \mathrm{if}\ \beta_i = 0
\end{cases}.
$$

Then, if we define the $i$-th column of $X$ to be $X_i$, we have the Lasso optimality condition as

$$
\begin{cases}
    X_i^T(y - X\beta) = \lambda\mathrm{sign}(\beta_i) & \mathrm{if}\ \beta_i \neq 0\\
    |X_i^T(y - X\beta)| \leq \lambda & \mathrm{if}\ \beta_i = 0
\end{cases}.
$$

### Soft-thresholding

The soft-thresholding problem is simply the Lasso problem with $X = I$:

$$
\min_\beta \frac{1}{2}\|y - \beta\|_2^2 + \lambda\|\beta\|_1.
$$

Then, we have the optimality condition as

$$
\begin{cases}
   y_i - \beta_i = \lambda\mathrm{sign}(\beta_i) & \mathrm{if}\ \beta_i \neq 0\\
   |y_i - \beta_i| \leq \lambda & \mathrm{if}\ \beta_i = 0
\end{cases}.
$$

Define the soft-thresholding operator $S_\lambda(y)$ as

$$
[S_\lambda(y)]_i = \begin{cases}
    y_i - \lambda & \mathrm{if}\ y_i > \lambda\\
    0 & \mathrm{if}\ -\lambda \leq y_i \leq \lambda\\
    y_i + \lambda & \mathrm{if}\ y_i < -\lambda
\end{cases}.
$$

We can see that if we set $\beta = S_\lambda(y)$ the optimality condition is satisfied.