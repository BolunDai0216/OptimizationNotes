# Proximal Gradient Descent 

The problem of interest has the form of

$$
\min_x g(x) + h(x)
$$

where $g(x)$ is convex and differentiable and $h(x)$ is convex while not necessarily differentiable. Recall when discussing gradient descent we would find a quadratic approximization to the objective function $f(x)$

$$
\tilde{f}_t(z) = f(x) + \nabla f(x)^T(z - x) + \frac{1}{2t}\|z - x\|_2^2
$$

then we take the step such that this quadratic approximization is minimized

$$
x^+ = x - t\nabla f(x).
$$

The idea here is since only $g$ is differentiable, why not only make a quadratic approximization to $g$ and leave $h$ alone? If that is the case we have

$$
\begin{align}
x^+ &= \underset{z}{\mathrm{argmin}}\Big(\tilde{g}_t(z) + h(z)\Big)\\
    &= \underset{z}{\mathrm{argmin}}\Big(g(x) + \nabla g(x)^T(z - x) + \frac{1}{2t}\|z - x\|_2^2 + h(z)\Big)\\
    &= \underset{z}{\mathrm{argmin}}\Big(\frac{1}{2t}\Big\|z - [x - t\nabla g(x)]\Big\|_2^2 + h(z)\Big)
\end{align}
$$(eqn:proximal_gd_without_prox_op)

If we define the proximal mapping as 

$$
\mathrm{prox}_t(x) = \underset{z}{\mathrm{argmin}}\Big(\frac{1}{2t}\|x - z\|_2^2 + h(z)\Big)
$$

Then, we can write the operations in {eq}`eqn:proximal_gd_without_prox_op` as 

$$
x^{(k)} = \mathrm{prox}_{t_k}\Big(x^{(k-1)} - t_k\nabla g(x^{(k-1)})\Big).
$$

To make it look more familiar, we can use the generalized gradient

$$
G_t(x) = \frac{x - \mathrm{prox}_{t}\Big(x - t\nabla g(x)\Big)}{t}
$$

which makes the operations in {eq}`eqn:proximal_gd_without_prox_op` look like

$$
x^{(k)} = x^{(k-1)} - t_kG_{t_k}(x^{(k-1)}).
$$

## Iterative Soft-thresholding Algorithm (ISTA)

For the Lasso problem

$$
\min_\beta \underbrace{\frac{1}{2}\|y - X\beta\|_2^2}_{g(\beta)} + \underbrace{\lambda\|\beta\|_1}_{h(\beta)}
$$

we can write it in proximal gradient descent form $\min_\beta g(\beta) + h(\beta)$. We can then get the update as

$$
\begin{align}
    \beta^+ &= \underset{z}{\mathrm{argmin}}\Big(\tilde{g}_t(z) + h(z)\Big)\\
            &= \underset{z}{\mathrm{argmin}}\Big(\frac{1}{2t}\Big\|z - (\beta - t\nabla g(\beta))\Big\|_2^2 + h(z)\Big)\\
            &= \underset{z}{\mathrm{argmin}}\Big(\frac{1}{2}\Big\|z - [\beta - t\nabla g(\beta)]\Big\|_2^2 + t\lambda\|z\|_1\Big)
\end{align}
$$

Let the proximal mapping be 

$$
\begin{align}
\mathrm{prox}_t(x) &= \underset{z}{\mathrm{argmin}}\frac{1}{2t}\|x - z\|_2^2 + \lambda\|z\|_1\\
                   &= \underset{z}{\mathrm{argmin}}\frac{1}{2}\|x - z\|_2^2 + t\lambda\|z\|_1\\
                   &= \underset{z}{\mathrm{argmin}}\frac{1}{2}\|z - x\|_2^2 + t\lambda\|z\|_1\\
                   &= S_{t\lambda}(x)
\end{align}
$$

where $S_{t\lambda}(x)$ is the soft-thresholding operator. Using the fact that $\nabla g(\beta) = -X^T(y - X\beta)$, we can then write the update using the soft-thresholding operator as

$$
\beta^+ = S_{t\lambda}(\beta - t\nabla g(\beta)) = S_{t\lambda}(\beta + tX^T(y - X\beta)).
$$

This is called the Iterative Soft-thresholding Algorithm (ISTA). In the general case of

$$
\min_\beta g(\beta) + \lambda\|\beta\|_1
$$

where $g$ is convex and differentiable, we have the update as $\beta^+ = S_{t\lambda}(\beta - t\nabla g(\beta))$.

## Fast Iterative Soft-thresholding Algorithm (FISTA)

FISTA is simply ISTA with momentum, the iteratives would have the form of

- Set $\beta^{(0)} = \beta^{(-1)} \in \mathbb{R}^n$;
- Momentum step: $\displaystyle v = \beta^{(k-1)} + \frac{k-2}{k+1}\Big[\beta^{(k-1)} - \beta^{(k-2)}\Big]$;
- ISTA update: $\beta^{(k)} = S_{t_k\lambda}(v - t_k\nabla g(v))$;
- Check convergence, if not go back to step 2;
