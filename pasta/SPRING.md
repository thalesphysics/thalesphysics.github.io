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

Which, in summary, can be written as:

$$
\frac{d^2}{dt^2}\vec{x} = - \frac{k}{m} M \vec{x}
$$

This certainly resembles a harmonic system, albeit in matrix form. It makes sense to consider a complex exponential solution, where the physical solution would be given by the real part of the exponential. Furthermore, we can assume solutions such that the frequency of motion for all masses is the same. A solution of this type can be described by the vector:

$$
\vec{x}(t) = e^{i \omega t}\begin{bmatrix} 
a_0 \\ a_1 \\ a_2 \\ \vdots \\ a_N 
\end{bmatrix} 
$$

Thus, substituting it into our matrix equation:

$$
\frac{d^2}{dt^2}e^{i \omega t}\vec{a} = - \frac{k}{m} M e^{i \omega t}\vec{a}
$$

$$
- \omega^2 e^{i \omega t}\vec{a} = - \frac{k}{m} M e^{i \omega t}\vec{a}
$$

Simplifying and renaming the variables:

$$
\lambda = \omega^2
$$

Yields:

$$
\frac{k}{m} M \vec{a} = \lambda \vec{a}
$$

Therefore, the problem is reduced to finding the eigenvalues and eigenvectors of the matrix.

## Solving with Python

The computational routine developed to solve this system is notably straightforward. It is structured into the following four stages:

* **Variable Initialization**
* **Matrix Construction**
* **Solving The Eigensystem**
* **Data Visualization**


```python

import numpy as np                          # Import the libraries
import matplotlib.pyplot as plt

k = 1                                       # Set the spring constant
m = 1                                       # Set the mass
N = 1000                                    # Set the number of masses

M = np.zeros((N,N))                         # Set a null matrice

for i in range(1,N-1):                      # Fill the matrice
    M[i, i] = -2
    M[i, i+1] = 1
    M[i, i-1] = 1

M[0,0] = -2
M[0,1] = 1
M[N-1,N-2] = 1
M[N-1,N-1] = -2

M *= -k/m                                  # Multiply by the constant
print(M)                                   # Print the matrice

l, v = np.linalg.eig(M)                    # Find the eigenvectors and eigenvalues

idx = np.argsort(l)                        # Sort the eigenvectors and eigenvalues lists

l = l[idx]
v = v[:, idx]

plt.plot(range(N),v[:,0])                  # Plot the displacements of the first normal mode
plt.show()

```
Running the code, you should obtain the following image:

<div style="display: flex; align-items: flex-start; gap: 40px;">
  <div>
    <img src="../pics/AmplitudePerIndex.png" style="width: 1000px; max-width: 200%; border-radius: 8px;" alt="Amplitude per Index">
  </div>
</div>

This image closely resembles half a period of a sine function. A more thorough analytical investigation would show that the amplitude is indeed a sinusoidal function of the mass index, in direct analogy with the stationary solutions of the wave equation for a vibrating string.

## Making a Visualization with Godot

### The Mass Sprite

To begin creating a visualization in Godot, we first need an image to represent the masses. This can be done either by importing any image from the internet or, more conveniently—as done here—by creating a sprite and assigning it a 2D gradient texture.

<div style="display: flex; align-items: flex-start; gap: 40px;">
  <div>
    <img src="../pics/massgodot.png" style="width: 1000px; max-width: 200%; border-radius: 8px;" alt="Mass setup">
  </div>
</div>

### The Godot Code

The simplest structure of our code does not involve particularly complex concepts, since it is initially intended solely for visualizing the motion of the masses.

The code performs the following tasks:
- Defines constant variables such as the amplitude scale, frequency, distance between masses, the number of masses, and the vertical position of the masses;
- Defines a list to store the mass objects and another list for their corresponding amplitudes;
- Defines a variable to keep track of time;
- When the simulation starts, \(N\) masses are created;
- At each frame, the position of each mass is updated according to its relative amplitude.

```gdscript
extends Node2D

var N = 3                                                      # Set the number of masses

var masses = []                                                # Set the masses array

var amplitude_factor = 100                                     # Set a factor of amplitude
var omega = 10                                                 # Set the agular frequency of oscilation
var distance = 300                                             # Set the distance between masses
var y0 = 500                                                   # Set the vertical position

var amplitudes = [1,-1,1]                                      # Set the relative amplitudes array

var t = 0                                                      # Set the time variable

func _ready():
	for i in range(N):                                           # Create N masses and set their vertical position when the simulation starts
		var mass = load("res://mass.tscn").instantiate()
		add_child(mass)
		mass.global_position.y = y0
		masses.append(mass)

func _physics_process(delta):                                  # Update position of all masses each frame
	for i in range(N):
		masses[i].global_position.x = (i+1) * distance + amplitude_factor * amplitudes[i] * sin(omega*t)
	t += delta
```
