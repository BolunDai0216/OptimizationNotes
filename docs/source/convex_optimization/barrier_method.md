# Barrier Method

Barrier method is a technique that transforms inequality constraints into the objective function. Then we can solve an unconstrained or only equality constrained optimization problem.

## Barrier Functions

The type of problem that we intend to solve using Barrier methods are in the form of

$$
\begin{align}
\min_x\ &\ f(x)\\
\mathrm{subject\ to}\ &\ h_i(x) \leq 0,\ i = 1, \cdots, m\\
                      &\ Ax = b. 
\end{align}
$$(eqn:inequality_constrained_opt_problem)

First, we temporarily ignore the equality constraint, which makes the problem become

$$
\begin{align}
\min_x\ &\ f(x)\\
\mathrm{subject\ to}\ &\ h_i(x) \leq 0,\ i = 1, \cdots, m. 
\end{align}
$$(eqn:only_inequality_constrained_opt_problem)

Then, if we replace the inequality constraint with the indicator function, which is defined as

$$
I_{\{h_i(x) \leq 0\}} = \begin{cases}
0 & h_i(x) \leq 0\\
+\infty & h_i(x) > 0,
\end{cases}
$$

we have the optimization problem as

$$
\min_x\ f(x) + \sum_{i=1}^{m}{I_{\{h_i(x) \leq 0\}}}.
$$(eqn:opt_problem_with_indicator_function)

Note that this transforms the inequality constrained optimization problem in {eq}`eqn:only_inequality_constrained_opt_problem` to an unconstrained optimization problem, which can be solved using Newton's method. However, given that the gradient of the indicator function is either zero or infinity, it would not be numerical friendly to solve this problem. We can then use a log barrier function to approximate the indicator function

$$
\mathrm{Log\ Barrier\ Function} := \frac{1}{t}\log(-h_i(x)).
$$

:::{figure-md} log_barrier_functions

<img src="../images/log_barrier_functions.png" alt="log barrier functions" class="bg-primary" width="600px">

Log barrier functions under different values of $t$. [[notebook](../notebooks/BarrierMethod.ipynb)]
:::

We can see that the larger $t$ is the more accurate the approximation becomes.

## Algorithm

We transform the problem in {eq}`eqn:opt_problem_with_indicator_function` using log barrier functions to

$$
\min_x\ f(x) + \sum_{i=1}^{m}{\frac{1}{t}\log(-h_i(x))}.
$$

Only common trick is that we multiply $t$ to the objective function and write it as

$$
\min_x\ tf(x) + \sum_{i=1}^{m}{\log(-h_i(x))}
$$

which has the same optimizer. And we can add back the equality constraint

$$
\begin{align}
\min_x\ &\ tf(x) + \sum_{i=1}^{m}{\log(-h_i(x))}\\
\mathrm{subject\ to}\ & Ax = b. 
\end{align}
$$(eqn:inequality_constrained_opt_problem_with_log_barrier)

And we have the optimization algorithm as

1. an initial starting point $x^{\{0\}}$ and $t^{\{0\}}$ would need to be provided;
2. solve the problem in {eq}`eqn:inequality_constrained_opt_problem_with_log_barrier` using Newton's method and set the solution $x^\star \rightarrow x^{\{k\}}$;
3. if $m/t \leq \epsilon$ stop, else set $t^{\{k+1\}} \leftarrow \mu t^{\{k\}}$ and go back to step 2.

This gives us our first method for solving an inequality constrained optimization problem.

