---
title: The Intuition of Natural Gradient
created: "2022-12-31"
modified: "2023-01-03"
image: assets/images/wng_14_0.png
---

In this post, I will discuss the intuition of the natural gradient methods. This post is largely based on `amari1998why`[^amari1998why]  with a simple example of a quadratic function.

## Riemannian Geometry without Tears

In the Euclidean geometry, we measure the length of a curve $$u: \mathbb R \to \mathbb R^{d}$$ by 

$$ \int_a^b \| \nabla u(t) \|_2 \dd t = \int_a^b  \sqrt{ \nabla u(t)^\top \nabla u(t)} \dd t, $$

where $$\nabla$$ means the element-wise (Jacobian) derivatives in $$\mathbb R^d$$.

The Riemannian geometry extends this by 

$$ \int_a^b \| \nabla u(t) \|_{\color{BrickRed}G} \dd t := \int_a^b  \sqrt{ \nabla u(t)^\top {\color{BrickRed}G} \nabla u(t) } \dd t, $$

where $$G \in \mathbb R^{d\times d}$$ is a symmetric positive definite matrix. The matrix distorts the direction of the gradient and emphasizes some directions more.  It is worth noting that the matrix $$G$$ can be different at different points $$ \in \mathbb R^d $$. In this case, it defines a ``local'' metric at each point for calculating the curvature.

## An Example where Parameterization Affects Optimization

Why do we need this new definition? Consider a very simple function in the Cartesian coordinates 

$$f(x, y) = (x-1)^2 + y^2.$$

The gradient of the function is straightforward

$$\nabla f(x, y) = \left(\begin{array}{c} 2 (x - 1) \\ 2  y \end{array} \right)$$

which is visualized below

![png]({{ site.assets }}/assets/images/wng_9_0.png)

We can see that the gradient points to the global minimum everywhere. This can be mathematically verified: $$\left( x - (x-1), y - y \right) = (1, 0) $$ is the only minimizer.

Now let's reparameterize this minimization problem in the polar coordinates $$(r, \theta)$$ with 

$$\begin{align*}
 x =& r \cos \theta, \\
y =& r \sin \theta.
\end{align*}$$

The objective function $$g$$ is 

$$\begin{align*}
g(r, \theta) =& (r\cos\theta - 1)^2 + (r \sin \theta)^2 \\
=& r^2 \underbrace{(\cos^2 \theta + \sin^2 \theta)}_{=1} -  2 r \cos \theta + 1 \\
=& r^2 -  2 r \cos \theta + 1.
\end{align*}$$

The gradient is

$$\nabla g(r, \theta) = \left(\begin{array}{c} 2r - 2 \cos \theta \\
 2 r \sin \theta \end{array}\right),$$

which is visualized below

![png]({{ site.assets }}/assets/images/wng_11_0.png)

As shown in the figure, the gradient does not necessarily points to the minimizer.

These two cases suggest that the gradients of different parameterizations may be so different, even if the functions essentially represent the same thing.

What can we do about it? We observe that the polar coordinates are ``distorted'' from the original representation. If we can find the matrix $$G$$ to recover the metric in the original space, we can then correct the gradient direction by setting it to the steepest one in the original space rather than the current one.

## Finding $$G$$

Recall the definition of length in the Riemannian geometry, we want to find a $$G$$ such that

$$\begin{align} (\nabla x(t))^2 + (\nabla y(t))^2 = (\nabla r(t), \nabla  \theta(t))^\top G (\nabla r(t), \nabla \theta(t)). \label{eq:conn}\end{align}$$

By the relationships of $$(r, \theta)$$ and $$(x, y)$$, we have $$\nabla x = \nabla_r x \nabla r + \nabla_\theta x \nabla \theta$$ and $$\nabla y = \nabla_r y \nabla r +  \nabla_\theta y \nabla\theta$$,

where

$$\begin{align*}
\nabla_r x =& \frac{\partial x}{\partial r} = \frac{\partial ( r \cos \theta ) }{ \partial r}  = \cos \theta  \\

\nabla_\theta x =& \frac{\partial x}{\partial \theta}   = \frac{\partial ( r \cos \theta ) }{ \partial \theta}  = - r \sin \theta  \\

\nabla_r y =& \frac{\partial y}{\partial r} = \frac{\partial ( r \sin \theta ) }{ \partial r}  = \sin \theta \\

\nabla_\theta y =& \frac{\partial y}{\partial \theta} = \frac{\partial ( r \sin \theta ) }{ \partial \theta}  = r \cos \theta.
\end{align*}$$

This gives us 

$$\begin{align*}
\nabla x = \cos \theta \nabla r - r \sin \theta \nabla \theta \\
\nabla y = \sin \theta \nabla r + r \cos \theta \nabla \theta
\end{align*}
$$

The left-hand side of Equation \eqref{eq:conn} is then 

$$\begin{align*}
LHS =& \left( \cos^2 \theta + \sin^2 \theta \right) (\nabla r)^2  + \\
 & \left(2 r \sin \cos  - 2 r \sin \cos  \right) \nabla r \nabla \theta  +\\
& \left(r^2 \sin^2 \theta  + r^2 \sin^2 \theta \right) (\nabla \theta)^2  \\
=& (\nabla r)^2 + r^2  (\nabla \theta)^2
\end{align*}$$

It is now clear that 

$$
G = \left( \begin{array}{cc} 1 & 0 \\  0 & r^2 \end{array} \right)
$$

Wow, it is actually quite easy to compute! Note that the matrix $$G$$ depends on the point in the space, as we mentioned previously. This again shows that the distortion may be different at different points.

##  Finding the Steepest Direction 

We now know how the polar space is distorted from the Cartesian space, but how can we find the correct direction as in the original space? We need to go back to the length again.

Recall that the direction of the original gradient descent is 

$$
\lim_{\mu \to 0} \mathop{\arg\max}_{\| d \|_2 \le 1} h(x + \mu d).
$$

Since we have the new system to measure the distance d, the formulation is now 

$$
\lim_{\mu \to 0} \mathop{\arg\max}_{\| d \|_G \le 1} h(x + \mu d).
$$

In `ollivier2017`[^ollivier2017], they show that the direction is aligned with

$$
\tilde \nabla h(x) := G^{-1} \nabla h(x),
$$

which is our "natural gradient".

In our example, the natural gradient of $$g(r, \theta)$$ is then 

$$\begin{align*}
\tilde \nabla g(r, \theta) =& G^{-1} \nabla g(r, \theta) \\
=& \left( \begin{array}{cc} 1 & 0 \\  0 & \frac{1}{r^2} \end{array} \right) \left(\begin{array}{c} 2r - 2 \cos \theta \\
 2 r \sin \theta \end{array}\right) \\
=&  \left(\begin{array}{c} 2r - 2 \cos \theta \\
 2  \sin \theta / r \end{array}\right) 
\end{align*}$$

We show the comparison of the vanilla gradient and natural gradient below (in the polar space for clarity).

![png]({{ site.assets }}/assets/images/wng_14_0.png)

Unlike the original gradient, the natural gradient is again pointing to the minimizer.

## Experiments

![png]({{ site.assets }}/assets/images/wng_17_0.png)

With the learning rate set as $$0.5$$, the natural gradient achieves the minimum in one step, whereas the vanilla gradient needs more iterations.

Hope you enjoy the read and are convinced by the derivation of the the natural gradient methods.

[^amari1998why]:
    ```bibtex
    @inproceedings{amari1998why,
      title = {Why natural gradient?},
      author = {Amari, S. and Douglas, S.C.},
      booktitle = {ICASSP},
      year = {1998},
      url = {https://ieeexplore.ieee.org/document/675489},
      pages = {1213-1216}
    }
    ```
    [Why Natural gradient](https://ieeexplore.ieee.org/document/675489)

[^ollivier2017]:
    ```bibtex
    @article{ollivier2017information,
      author  = {Yann Ollivier and Ludovic Arnold and Anne Auger and Nikolaus Hansen},
      title   = {Information-Geometric Optimization Algorithms: A Unifying Picture via Invariance Principles},
      journal = {JMLR},
      year    = {2017},
      volume  = {18},
      number  = {18},
      pages   = {1--65},
      url     = {http://jmlr.org/papers/v18/14-467.html}
    }
    ```
    [Information-Geometric Optimization Algorithms: A Unifying Picture via Invariance Principles](http://jmlr.org/papers/v18/14-467.html)
