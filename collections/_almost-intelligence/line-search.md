---
title: Line Search Methods
created: "2023-02-15"
modified: "2023-02-15"
---

{: .quote }
[The learning rate] is often the single most important hyperparameter and one should always make sure that it has been tuned. --- Yoshua Bengio


## Introduction

The optimization iteration is given by 

$$x_{k+1} \gets x_k - \alpha_k p_k,$$

where $$\alpha_k \in \mathbb R$$ is the learning rate and $$p_k \in \mathbb R^d$$ is the direction. Generally speaking, the direction is obtained by 

$$p_k :=  B_k^{-1} \nabla f(x)$$

where $$B_k$$ is a symmetric and nonsingular matrix. If $$B_k$$ is the identity matrix, then the direction is the negative gradient. If $$B_k$$ is the Hessian matrix, then the direction is the Newton direction.

When $$\nabla f(x_k)^\top  p_k > 0$$, the direction $$p_k$$ is a descent direction. This implies that 

$$\nabla f(x_k)^\top p_k = \nabla f(x_k)^\top B_k^{-1} \nabla f(x_k)  > 0.$$

In this post, we will discuss the line search methods for finding the optimal learning rate $$\alpha_k$$ given the direction $$p_k$$.


Ideally, the line search methods should satisfy the following properties:

$$\alpha_k = \mathop{\arg\min}_\alpha f(x_k - \alpha p_k),$$

where $$\alpha \in \mathbb R$$ is the learning rate. However the above equation is not easy to solve. In practice, we use the inexact line search methods to find the optimal learning rate. One simple heuristic is to enforce the deacreasing property of the objective function:

$$f(x_k - \alpha_k p_k) < f(x_k)$$

However, this does not guarantee to find the optima of the objective function. 

{: .example }
Let $$f(x) := x^2 - 1$$. If the line search algorithm generates a sequence of learning rates $$\alpha_k$$ such that $$f(x_k) = 1/(k+1)$$, then the algorithm will not find the optimal solution $$f^* = -1$$.

The example suggests we need more sophisticated conditions to ensure the line search methods find the more optimal solution.

## Armijo Condition

The Armijo condition is given by 

$$\begin{align}
f(x_k - \alpha_k p_k) \leq f(x_k) - \gamma \alpha_k \nabla f(x_k)^\top p_k,
\end{align}$$

where $$\gamma \in (0, 1)$$ is a constant. The Armijo condition is a sufficient condition for the line search methods to find the optimal solution. 


## Wolfe Conditions

The Wolfe conditions are given by 

$$\begin{align}
f(x_k - \alpha_k p_k) \leq f(x_k) - \gamma \alpha_k \nabla f(x_k)^\top p_k, \\
\nabla f(x_k - \alpha_k p_k)^\top p_k \leq \delta \nabla f(x_k)^\top p_k,
\end{align}$$

where $$\gamma \in (0, 1)$$ and $$\delta \in (0, 1)$$ are constants. 

