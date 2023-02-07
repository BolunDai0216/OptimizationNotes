# Fundametals of Dual Method

This part of the notes will discuss dual methods, e.g. dual (sub)gradient ascent, augmented Lagrangian (AL), and alternating direction method of multipliers (ADMM). This note will first present some fundametal properties of dual methods, and then show how dual (sub)gradient ascent works along with its distributed version. For AL and ADMM, we will discuss about them in later notes.

## Motivation

As the name suggests, dual methods attempt to solve the primal problem by solving the dual problem. If we take a convex optimization problem with equality constraints as example

$$
\begin{align}
    \min_x\ &\ f(x)\\
    \mathrm{subject\ to}\ &\ Ax = b.
\end{align}
$$(eqn:dual_method_motivating_example)

We can have its Lagrangian as

$$
L(x, u) = f(x) + u^T(Ax - b)
$$

and the Lagrangian dual function as

$$
g(u) = \min_x\Big(f(x) - (-u^TA)x - u^Tb\Big).
$$(eqn:dual_method_lagrangian_dual)

Define the conjugate function of $f(x)$ as

```{admonition} Conjugate Function
Given $f:\mathbb{R}^n \rightarrow \mathbb{R}$, the conjugate function of $f$ is define as

$$
f^*(y) = \max_{x}\Big(y^Tx - f(x)\Big) 
$$(eqn:conjugate_function)

also it can be defined as posing it as a minimization problem

$$
-f^*(y) = \min_{x}\Big(f(x) - y^Tx\Big). 
$$
```

Then we can we write {eq}`eqn:dual_method_lagrangian_dual` using conjugate functions:

$$
\begin{align}
g(u) &= \min_x\Big(f(x) - (-u^TA)x\Big) - u^Tb\\
     &= -f^*(-A^Tu) - u^Tb.
\end{align}
$$(eqn:dual_method_lagrangian_dual_conjugate_form)

This gives us a succinct way of describing the Lagrangian dual function. The dual methods are motivated by situations where the conjugate function cannot be obtained in close form. In those cases, we can still solve the optimization problem using dual-based subgradient or gradient methods.

## Properties of Conjugate Functions

There are four main properties of conjugate functions that are useful to our analysis of dual methods:

```{admonition} Property 1
If $f$ is closed and convex then $f^{**} = f$.
```

```{admonition} Property 2
The following statements are equivalent
- $x \in \partial f^*(y)$;
- $y \in \partial f(x)$;
- $x \in \underset{z}{\mathrm{argmin}}(f(z) - y^Tz)$.
```

```{dropdown} Proof of Property 2
**Part 1.1**: We show that $y \in \partial f(x) \Rightarrow x \in \partial f^*(y)$   

From the definition of conjugate functions we know

$$
-f^*(y) = \min_z\Big(f(z) - z^Ty\Big).
$$

Thus, using the subgradient optimality condition, for any $z$ that is optimal it must satisfy

$$0\in\partial_z(f(z) - z^Ty) \Leftrightarrow 0\in\partial f(z) - y \Leftrightarrow y\in\partial f(z).$$

Also for any $z$ that satisfies $y\in\partial f(z)$ it must be optimal. Since $y \in \partial f(x)$, then $z = x$ must minimize $f(z) - z^Ty$ over $z$. Define

$$
f^*(y) := \max_z\Big(z^Ty - f(z)\Big) := \max_z h_z(y)
$$

Now we need to acknowledging one of the properties of general pointwise maximums: if $f(x) = \max_{s\in\mathcal{S}}f_s(x)$, then

$$
\mathrm{cl}\Big\{\mathrm{conv}\Big(\bigcup_{s: f_s(x) = f(x)}\partial f_s(x)\Big)\Big\}\subseteq\partial f(x).
$$

Note that $\partial h_z(y) = z$. Then, by the definition of general pointwise maximums, we have

$$
\mathrm{cl}\Big\{\mathrm{conv}\Big(\bigcup_{z: z\mathrm{s\ that\ achieve\ the\ max}} z\Big)\Big\}\subseteq\partial f^*(y).
$$

Hence, we have $x \in \partial f^*(y)$.

**Part 1.2** We show that $y \in \partial f(x) \Leftarrow x \in \partial f^*(y)$   

From the above derivation we know that $y \in \partial f(x) \Rightarrow x \in \partial f^*(y)$. Then, we can conclude that $x \in \partial f^*(y) \Rightarrow y \in \partial f^{**}(x)$, given that $f = f^{**}$, we have $x \in \partial f^*(y) \Rightarrow y \in \partial f(x)$.

**Part 2.1** We show that $\displaystyle y \in \partial f(x) \Rightarrow x \in \underset{z}{\mathrm{argmin}}(f(z) - y^Tz)$   

The subgradient optimality condition for $f(z) - y^Tz$ is $0 \in \partial_z(f(z) - y^Tz)$, which can be written as $y \in \partial_z f(z)$. Since $y \in \partial f(x)$, then $x$ must minimize $f(z) - y^Tz$.

**Part 2.2** We show that $\displaystyle y \in \partial f(x) \Leftarrow x \in \underset{z}{\mathrm{argmin}}(f(z) - y^Tz)$ 

If $x$ minimizes $f(z) - y^Tz$, then from the subgradient optimality condition we have $0 \in \partial f(x) - y$, i.e. $y \in \partial f(x)$.

Thus, we can see all three statements are equivalent.
```

```{admonition} Property 3
Assuming that $f$ is closed and convex. Then, the following two statements are equivalent
- $f$ is strongly convex with parameter $d$;
- $\nabla f^*$ is Lipschitz with parameter $1/d$.
```

```{dropdown} Proof of Property 3
If we have a strongly convex function $g$ with parameter $d$, then it has the property

$$
g(y) \geq g(x) + \nabla g(x)^T(y - x) + \frac{d}{2}\|y - x\|_2^2
$$

if $x$ is a minimizer, i.e. $\nabla g(x) = 0$, then we have 

$$
g(y) \geq g(x) + \frac{d}{2}\|y - x\|_2^2,\ \forall y.
$$

If we define two functions

$$
\begin{align}
    g_1(x) &= f(x) - u^Tx\\
    g_2(x) &= f(x) - v^Tx
\end{align}
$$

then we have (from property 4)

$$
\begin{align}
    x_u^\star &= \underset{z}{\mathrm{argmin}}(f(x) - u^Tx) = \nabla f^*(u)\\
    x_v^\star &= \underset{z}{\mathrm{argmin}}(f(x) - v^Tx) = \nabla f^*(v).
\end{align}
$$

This leads to

$$
\begin{align}
    g_1(x_v) &\geq g_1(x_u) + \frac{d}{2}\|x_v - x_u\|_2^2\\
    g_2(x_u) &\geq g_2(x_v) + \frac{d}{2}\|x_v - x_u\|_2^2
\end{align}
$$

if we add them up we have $g_1(x_v) + g_2(x_u) \geq g_1(x_u) + g_2(x_v) + d\|x_v - x_u\|_2^2$, which is the same as 

$$
f(x_v) - u^Tx_v + f(x_u) - v^Tx_u \geq f(x_u) - u^Tx_u + f(x_v) - v^Tx_v + d\|x_v - x_u\|_2^2
$$

which can be cleaned up and get

$$
(u - v)^T(x_u - x_v) \geq d\|x_v - x_u\|_2^2.
$$

From Cauchy-Schwartz, we then can get 

$$
\|u - v\|_2\|x_u - x_v\|_2 \geq d\|x_v - x_u\|_2^2 \rightarrow \frac{1}{d}\|u - v\|_2 \geq \|x_v - x_u\|_2.
$$
```

```{admonition} Property 4
If $f$ is strictly convex, then $\nabla f^*(y) = \underset{z}{\mathrm{argmin}}(f(z) - y^Tz)$.
```

```{dropdown} Proof of Property 4
Since $f$ is strictly convex, then $f(z) - y^Tz$ would also be strictly convex over $z$. Then, for the strictly convex function $f(z) - y^Tz$ we would have a unique minimizer. TBC.
```

## Dual (Sub)Gradient Methods

From {eq}`eqn:dual_method_lagrangian_dual_conjugate_form`, we know that we can write the Lagrangian dual function using conjugate functions. Then, we have the Lagrangian dual problem as

$$
\max_{u}\ g(u) = \max_{u}\Big[-f^*(-A^Tu) - u^Tb\Big].
$$

We can have the subgradient of $g$ as

$$
\partial g(u) = A\partial f^*(-A^Tu) - b.
$$

From property 2, we have the relationship

$$
x\in\partial f^*(-A^Tu) \iff x\in\underset{z}{\mathrm{argmin}}\Big[f(z) + u^TAz\Big].
$$

Then, we can write the subgradient of $g$ as

$$
\partial g(u) = Ax - b\ \mathrm{where}\ x\in\underset{z}{\mathrm{argmin}}\Big[f(z) + u^TAz\Big].
$$

The steps for the **dual subgradient method** for maximizing the dual objective is as follows

```{admonition} Dual Subgradient Method
:class: tip

Start for an initial dual guess $u^{(0)}$, and repeats for $k = 1, 2, 3, \cdots$

- $x^{(k)} \in \underset{x}{\mathrm{argmin}}\Big[f(x) + (u^{(k-1)})^TAx\Big]$
- $u^{(k)} = u^{(k-1)} + t_k(Ax^{(k)} - b)$

where the step sizes $t_k$ are chosen using backtracing line search.
```

## Dual Decomposition

The problem in consideration for dual decomposition is

$$
\begin{align}
    \min_x\ &\ \sum_{i=1}^{B}{f_i(x_i)}\\
    \mathrm{subject\ to}\ &\ Ax = b
\end{align}
$$

which can also be written as

$$
\begin{align}
    \min_x\ &\ \sum_{i=1}^{B}{f_i(x_i)}\\
    \mathrm{subject\ to}\ &\  \sum_{i=1}^{B}{A_ix_i}= b
\end{align}
$$

where $x = [x_1, \cdots, x_B] \in \mathbb{R}^n$ with $x_i \in \mathbb{R}^{n_i}$ and $A = [A_1, \cdots, A_B]$ with $A_i \in \mathbb{R}^{m \times n_i}$. The first step of the dual subgradient method becomes

$$
x^{(k)} \in \underset{x}{\mathrm{argmin}}\Big[\sum_{i=1}^{B}{f_i(x_i^{(k-1)})} + (u^{(k-1)})^T\sum_{i=1}^{B}{A_ix_i^{(k-1)}}\Big]
$$

we can also write is as

$$
x_i^{(k)} \in \underset{x_i}{\mathrm{argmin}}\Big[f_i(x_i^{(k-1)}) + (u^{(k-1)})^TA_ix_i^{(k-1)}\Big],\ i = 1, \cdots, B.
$$

Then, we have the dual decomposition algorithm as

```{admonition} Dual Decomposition Method with Equality Constraints
:class: tip

Start for an initial dual guess $u^{(0)}$, and repeats for $k = 1, 2, 3, \cdots$

- $x_i^{(k)} \in \underset{x_i}{\mathrm{argmin}}\Big[f_i(x_i^{(k-1)}) + (u^{(k-1)})^TA_ix_i^{(k-1)}\Big],\ i = 1, \cdots, B$
- $\displaystyle u^{(k)} = u^{(k-1)} + t_k(\sum_{i=1}^{B}{A_ix_i^{(k)}} - b)$

where the step sizes $t_k$ are chosen using backtracing line search.
```

For the problem with inequality constraints

$$
\begin{align}
    \min_x\ &\ \sum_{i=1}^{B}{f_i(x_i)}\\
    \mathrm{subject\ to}\ &\  \sum_{i=1}^{B}{A_ix_i} \leq b
\end{align}
$$

we have the dual decomposition algorithm as

```{admonition} Dual Decomposition Method with Inequality Constraints
:class: tip

Start for an initial dual guess $u^{(0)}$, and repeats for $k = 1, 2, 3, \cdots$

- $x_i^{(k)} \in \underset{x_i}{\mathrm{argmin}}\Big[f_i(x_i^{(k-1)}) + (u^{(k-1)})^TA_ix_i^{(k-1)}\Big],\ i = 1, \cdots, B$
- $\displaystyle u^{(k)} = \Big[u^{(k-1)} + t_k(\sum_{i=1}^{B}{A_ix_i^{(k)}} - b)\Big]_+$

where the step sizes $t_k$ are chosen using backtracing line search and $(u_+)_i = \max\{0, u_i\}.$
```