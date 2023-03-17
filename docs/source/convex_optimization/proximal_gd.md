# Proximal Gradient Descent 

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

This is called the Iterative Soft-thresholding Algorithm (ISTA).

