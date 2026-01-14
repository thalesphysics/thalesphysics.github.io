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

### 
