# Newton's Method

Newton's method is originally a root-finding method for nonlinear equations, but in combination with optimality conditions it becomes the workhorse of many optimization algorithms.

## Root Finding Using Newton's Method

First, we show how we can use Newton's method to solve the problem of

$$
f(x) = 0.
$$

The procedure is actually fairly simple

1. an initial starting point $\hat{x} \leftarrow x_0$ would need to be provided
2. find a linear approximation to $f(x)$ at $\hat{x}$ using Taylor's expansion $\tilde{f}(x) = f(\hat{x}) + \nabla f(\hat{x})(x - \hat{x})$
3. Solve the problem $\tilde{f}(x) = 0$ and set the solution to $\hat{x}$
4. go back to step 2 if $f(\hat{x}) > \epsilon$.

For solving the problem of $f(x) = x^2 + 2x - 10 = 0$, the process looks like

:::{figure-md} newtons_method_illustration

<img src="../images/newtons_method_illustration.png" alt="newtons method illustration" class="bg-primary" width="800px">

The iterations for solving $f(x) = x^2 + 2x - 10 = 0$. [[notebook](../notebooks/NewtonsMethod.ipynb)]
:::
