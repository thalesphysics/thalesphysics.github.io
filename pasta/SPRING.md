---
title: "Physics Simulators"
layout: default
---

<style>
.site-header {
  display: none;
}
</style>


<head>
<style>
a {
  color: #59b390;
  text-decoration: none;
}
a:hover {
  color: #006400;
  text-decoration: underline;
}
</style>
</head>

<!-- Enables MathJax -->
<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

# [Run Simulation](../springs/index.html)

# The Coupled Mass-Spring System

A fundamental system in physics is the mass-spring system. This model can be generalized to cases where multiple masses and springs are interconnected. In this section, we will discuss its physical significance and establish a theoretical model. Furthermore, numerical solutions will be obtained using Python in conjunction with the Godot engine.

---

## Why Masses and Springs?

A mass and a spring attached to a wall—why study a system so simple and mundane? The truth is far more profound. Any system with stable equilibria can be reduced to a set of masses and springs.

Imagine, first, a one-dimensional system, such as a ball in a valley. If we assume the force acting on the ball is sufficiently smooth, its potential energy as a function of horizontal position can be described by a Taylor series:

$$
U(x) = U(x_o) + U'(x_o)(x-x_o) + \frac{U''(x_o)(x-x_o)^2}{2} + \dots
$$

It is evident that the bottom of the valley is a potential minimum, as gravitational potential energy is proportional to height, and the minimum height is at the valley floor. This indicates two important facts:

$$
U'(x_o) = 0
$$

$$
U''(x_o) > 0
$$

Furthermore, we can set the potential at the bottom of the well as our reference point, such that:

$$
U(x_o) = 0
$$

These results lead to a simplification of the expansion:

$$
U(x) = \frac{U''(x_o)(x-x_o)^2}{2} + \frac{U'''(x_o)(x-x_o)^3}{6} + \dots
$$

In many cases, we are not interested in the complete motion of the objects, but only in how that motion occurs around these points of minimum potential. This leads to a drastic simplification if we assume the displacements are small. By defining:

$$
U''(x_o) = k
$$

$$
x - x_o = \Delta x
$$

Thus:

$$
U(x) = \frac{k\Delta x^2}{2} + \mathcal{O}(\Delta x^3)
$$

Taking the limit:

$$
\Delta x \to 0
$$

The first non-zero term is the quadratic term in $\Delta x$. This reduces all calculations to:

$$
U(x) = \frac{k\Delta x^2}{2}
$$

This potential is, in fact, the potential of a spring, such that the force is simply proportional to the displacement:

$$
F(x) = -\frac{\partial U(x)}{\partial x} = -k \Delta x
$$
### More Masses

In a more comprehensive scenario involving a system of many interacting particles, it is still possible for one or more configurations to exist as local potential minima, which remain stable. In such cases, all forces can be approximated as spring forces. However, the force exerted by these springs depends on the displacement of two distinct particles. This creates a coupling within the system, making the equations of motion significantly more complex. Fortunately, specialized techniques exist to solve this type of system under specific conditions.

#### The Simplest One-Dimensional Coupled Mass-Spring System

To better understand this problem, we begin with the simplest case: a set of identical masses arranged in a line, connected to their immediate neighbors by identical springs. If we denote $x_i$ as the displacement of mass $i$ relative to its equilibrium point, the force acting on each particle is given by:



$$
F_i(\vec{x}) = m\ddot x_i = -k (x_i - x_{i+1}) - k (x_i - x_{i-1}) = k(x_{i+1} - 2x_i + x_{i-1})
$$

The forces acting on the first and last particles depend on the system's boundary conditions. In this instance, we will consider them to be connected to springs anchored to fixed walls. Thus:

$$
F_0(\vec{x}) = m\ddot x_0 = -k (x_0 - x_{1})
$$

$$
F_N(\vec{x}) = m\ddot x_N = -k (x_N - x_{N-1})
$$

Consequently, we have a set of $N$ coupled linear differential equations. This system can be rewritten in matrix form:

$$
m \begin{bmatrix} 
\ddot{x}_0 \\ \ddot{x}_1 \\ \ddot{x}_2 \\ \vdots \\ \ddot{x}_N 
\end{bmatrix} = -k 
\begin{bmatrix} 
2 & -1 & 0 & \dots & 0 \\
-1 & 2 & -1 & \dots & 0 \\
0 & -1 & 2 & \dots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & 0 & \dots & 2 
\end{bmatrix}
\begin{bmatrix} 
x_0 \\ x_1 \\ x_2 \\ \vdots \\ x_N 
\end{bmatrix}
$$

Que de maneira resumida pode ser escrita como

$$
\frac{d^2}{dt^2}\vec{x} = - \frac{k}{m} A \vec{x}
$$
