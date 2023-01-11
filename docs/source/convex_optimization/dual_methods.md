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
The note body will be hidden!
```

```{admonition} Property 3
Assuming that $f$ is closed and convex. Then, the following two statements are equivalent
- $f$ is strongly convex with parameter $d$;
- $\nabla f^*$ is Lipschitz with parameter $1/d$.
```

```{admonition} Property 4
If $f$ is strictly convex, then $\nabla f^*(y) = \underset{z}{\mathrm{argmin}}(f(z) - y^Tz)$.
```
