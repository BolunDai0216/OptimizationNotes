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
```

```{admonition} Property 3
Assuming that $f$ is closed and convex. Then, the following two statements are equivalent
- $f$ is strongly convex with parameter $d$;
- $\nabla f^*$ is Lipschitz with parameter $1/d$.
```

```{admonition} Property 4
If $f$ is strictly convex, then $\nabla f^*(y) = \underset{z}{\mathrm{argmin}}(f(z) - y^Tz)$.
```
