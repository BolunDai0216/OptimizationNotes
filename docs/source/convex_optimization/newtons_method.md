# Newton's Method

Newton's method is originally a root-finding method for nonlinear equations, but in combination with optimality conditions it becomes the workhorse of many optimization algorithms.

## Root Finding Using Newton's Method

First, we show how we can use Newton's method to solve the problem of

$$
f(x) = 0.
$$

The procedure is actually fairly simple

1. an initial starting point $\hat{x} \leftarrow x_0$ would need to be provided
2. find a linear approximation to $f(x)$ at $\hat{x}$ using Taylor's expansion $\widetilde{f}(x) = f(\hat{x}) + \nabla f(\hat{x})(x - \hat{x})$
3. Solve the problem $\widetilde{f}(x) = 0$ and set the solution to $\hat{x}$
4. go back to step 2 if $f(\hat{x}) > \epsilon$.

For solving the problem of $f(x) = x^2 + 2x - 10 = 0$, the process looks like

:::{figure-md} newtons_method_illustration

<img src="../images/newtons_method_illustration.png" alt="newtons method illustration" class="bg-primary" width="800px">

The iterations for solving $f(x) = x^2 + 2x - 10 = 0$. [[notebook](../notebooks/NewtonsMethod.ipynb)]
:::


## Solving Unconstrained Optimization Problems

The general form of unconstrained optimization problems is

$$
\min_x \quad f(x).
$$

### Pure Newton's Method

Under the assumption that $f(x)$ is convex, we have its optimality condition as

$$
\nabla f(x) = 0.
$$

Thus, if we use Newton's method to perform a root finding on the function $\nabla f(x)$, then we would find our optimal solution. A linear approximation to the gradient would be

$$
\nabla\widetilde{f}(x) = \nabla f(\hat{x}) + \nabla^2 f(\hat{x})(x - \hat{x}).
$$

And the solution to $\nabla\widetilde{f}(x) = 0$ is

$$
x = \hat{x} - \Big[\nabla^2 f(\hat{x})\Big]^{-1}\nabla f(\hat{x}).
$$

So, similar to the procedures for root finding, the steps to optimize an unconstrained convex optimization problems are

1. an initial starting point $\hat{x} \leftarrow x_0$ would need to be provided
2. find a linear approximation to $\nabla f(x)$ at $\hat{x}$ using Taylor's expansion $\nabla\widetilde{f}(x) = \nabla f(\hat{x}) + \nabla^2 f(\hat{x})(x - \hat{x})$
3. Solve the problem $\widetilde{f}(x) = 0$ and set the solution to $\hat{x} \leftarrow \hat{x} - \Big[\nabla^2 f(\hat{x})\Big]^{-1}\nabla f(\hat{x})$
4. go back to step 2 if $\nabla f(\hat{x}) > \epsilon$.

These are the steps to what is called "pure" Newton's method. However, what is commonly used right now is "damped" Newton's method, which is "pure" Newton's method with an additional backtracking line search.

### Backtracking Line Search

For pure Newton's method, we are performing a linear approximation to the gradient, that approximation would only be close to the actual gradient within a neighborhood of the current iteration. Thus, it would be reasonable to only rely on it in that neighborhood. Since locally that linear approximation does provide a direction where the gradient is closer to zero, we can take a step in that direction, but not the entire step as suggested by pure Newton's method. In this terminology, given the Newton's method update

$$
\hat{x} \leftarrow \hat{x} - t\Big[\nabla^2 f(\hat{x})\Big]^{-1}\nabla f(\hat{x}),\ t = 1
$$

the direction is $v = -\Big[\nabla^2 f(\hat{x})\Big]^{-1}\nabla f(\hat{x})$ and the step size is $t = 1$.

The steps for performing backtracking line search is as follows:

1. Set t = 1.
2. If $f(x + tv) > f(x) + \alpha t \nabla f^T(x)v$ go to step 3, else go to step 4.
3. Set $t = \beta t$ with $\beta\in(0,\ 1)$ and go to step 2.
4. Perform the Newton update.

If we take a look at the condition that needs to be checked

$$
f(x + tv) > f(x) + \alpha t \nabla f^T(x)v
$$(eqn:backtracking_line_search_cond)

The term $f(x + tv)$ denotes the value of the objective function after the proposed update. The value $t \nabla f^T(x)v$ is the expected decrease in objective function assuming we are:
- using a linearized model of the objective function;
- taking the step size $t$;
- following the direction v.

The term $\alpha$ is a discounting factor. Since for convex functions the gradient always is below the curve, without the discounting factor, the value of $f(x) + t \nabla f^T(x)v$ would always be less than $f(x + tv)$. The value of $\alpha$ is commonly chose to be 0.5.

Thus, the condition {eq}`eqn:backtracking_line_search_cond` is saying that taking the current step size is worse than the expected decrease, thus, it would be too large of a step, it should be reduced, which is exactly what $t = \beta t$ is doing.

### Damped Newton's Method

The damped Newton's method is simply a combination of pure Newton's method and backtracking line search. The steps are

1. an initial starting point $\hat{x} \leftarrow x_0$ would need to be provided;
2. compute the direction $v$;
3. find $t$ that satifies $f(x + tv) \leq f(x) + \alpha t \nabla f^T(x)v$ using backtracking line search;
4. set the solution to $\hat{x} \leftarrow \hat{x} + tv$;
5. go back to step 2 if $\nabla f(\hat{x}) > \epsilon$.

Damped Newton's method has distinctly two phase, one damped phase where the reduction for each step is similar and a quadratic convergence phase.

## Solving Equality Constrained Optimization Problems

The general form of equality constrained convex optimization problems is

$$
\begin{align}
\min_x\ &\ f(x)\\
\mathrm{subject\ to}\ &\ Ax = b.
\end{align}
$$

We can also see Newton's method as performing a quadratic approximation to the objective and find the minimum to this quadratic approximation. The quadratic approximation in this case is


$$
\widetilde{f}(x) = f(\hat{x}) + \nabla f^T(\hat{x})(x - \hat{x}) + \frac{1}{2}(x - \hat{x})^T\nabla^2f^T(\hat{x})(x - \hat{x})
$$

For equality constrained convex optimization problems, we require Newton's method to start at a feasible point, thus, we have $A\hat{x} = b$. We also want the point reached after each step to be feasible. Therefore, we are solving the following problem at each time step

$$
\begin{align}
\min_x\ &\ f(\hat{x}) + \nabla f^T(\hat{x})(x - \hat{x}) + \frac{1}{2}(x - \hat{x})^T\nabla^2f^T(\hat{x})(x - \hat{x})\\
\mathrm{subject\ to}\ &\ Ax = b.
\end{align}
$$

The solution to this problem $x^\star$ will give us the direction $v = x^\star - x$. Note that 

$$
Ax^\star = Av + Ax = Av + b = b.
$$

Therefore, we require $Av = 0$. Then, we can rewrite the approximate optimization problem solved at each time step as

$$
\begin{align}
\min_v\ &\ \nabla f^T(\hat{x})v + \frac{1}{2}v^T\nabla^2f^T(\hat{x})v\\
\mathrm{subject\ to}\ &\ Av = 0.
\end{align}
$$

The Lagrangian for this problem is

$$
L(v, w) = \nabla f^T(\hat{x})v + \frac{1}{2}v^T\nabla^2f^T(\hat{x})v + w^TAv.
$$

If we write the KKT conditions for this problem we have the stationarity condition as

$$
\nabla f(\hat{x}) + \nabla^2f^T(\hat{x})v + A^Tw = 0
$$

and we have the primal feasibility condition as

$$
Av = 0.
$$

If we write them together, we have

$$
\begin{bmatrix}
\nabla^2f^T(\hat{x}) & A^T\\
A & \mathbf{0}
\end{bmatrix}\begin{bmatrix}
v\\
w
\end{bmatrix} = \begin{bmatrix}
-\nabla f(\hat{x})\\
\mathbf{0}
\end{bmatrix}
$$

Thus, by solving this KKT system we would obtain the update direction $v$, and then we can do a backtracking line search to find the correct step size, and everything else would be the same as the unconstrained damped Newton's method.
