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

# [Run Simulation](../electro2d/index.html)

# 2D Electromagnetism

Electromagnetism in two dimensions is rewritten analogously to the three-dimensional model, but the laws and equations are still different. Thus, the content presented here involves a numerical modeling of Maxwell's equations to simulate the motion of point charges and the electric and magnetic fields in space. The entire theoretical model of Maxwell's equations rewritten for a two-dimensional world is based on the article "On Maxwell’s Electrodynamics in Two Spatial Dimensions", while the computational model is original. While the theory behind the equations can be somewhat complex, applying it in this simulation only requires basic knowledge of differential equations, and the implementation difficulty increases progressively.

## Point Charges and the Electric Field

### Gauss's Law

In two dimensions, Gauss's law takes a similar form, but both the charge density and the divergence must also be calculated in two dimensions.

$$
\vec{E} \equiv (E_x, E_y)
$$

$$
\nabla \equiv \frac{\partial E_x}{\partial x} + \frac{\partial E_y}{\partial y}
$$

$$
\rho(x,y) \equiv \frac{dq}{dA}
$$

Gauss's law becomes:

$$
\nabla \cdot \vec{E} = \frac{\rho}{\epsilon_o}
$$

The charge density of a point charge can be modeled as a generalized function called the Dirac delta, which depends only on position and returns zero at every point in space except at the origin, where it returns infinity.

$$
\delta(\vec{r}) = 
\begin{cases}
\infty & \text{if } \vec{r} \neq \vec{0} \\
0             & \text{if } \vec{r} = \vec{0}
\end{cases}
$$

$$
\rho(\vec{r}) = q\delta(\vec{r})
$$

From this, we can deduce the expression for the electric field of a point charge. Integrating both sides over a circular area centered on our point charge, and using Gauss’s theorem in two dimensions to convert the area integral into a line integral along the circumference, we obtain a closed-form expression for the field of the charge:

$$
\int \int_C \nabla \cdot \vec{E} ~dA = \frac{\int \int_C \rho ~dA}{\epsilon_o} 
$$

$$
\oint_{\partial C} \vec{E} \cdot \hat{n} ~dr  = \frac{\int \int_C q\delta(\vec{r}) ~dA}{\epsilon_o} 
$$

$$
E \oint_{\partial C} ~dr  = \frac{q}{\epsilon_o} 
$$

$$
E (2 \pi |r|)  = \frac{q}{\epsilon_o} 
$$

$$
E = \frac{q}{2 \pi \epsilon_o |r|} 
$$

### Godot Setup

To begin writing our code, we need to have a few resources available in the editor. Therefore, we need to:

* Create a root node  
* Import an image of a positive charge  
* Import an image of a negative charge  
* Import an image of an arrow  

![Godot Charge Setup](../pics/charge_setup.png)

### Creating a Vector Field in Godot

To clearly visualize how the electric field changes when a charge is placed in a region of space, we need two pieces of information. First, the direction of the electric field must be defined at every point in space. Second, the field's intensity at each point. This will be represented by creating a grid of arrows pointing in the direction of the field at that point, with the arrow’s transparency indicating the field's intensity.

#### Variables

We start by creating the variables that define the region and the number of points in the grid we want to generate.

```gdscript
extends Node2D     # Object type of root node

var v_field = []   # Array of our vectors

var nx = 60        # Number of horizontal points
var ny = 30        # Number of vertical points
var lx = 2000      # Region size in x
var ly = 1300      # Region size in y
```

#### Creating the Arrows

To generate the arrows in the desired manner, we iterate over each point in our vector field, create an image object, define its texture, scale, and position, and then add it to the scene. Simultaneously with this arrow creation process, we also add these arrows to our matrix.


```gdscript
func _ready():
	gen_e_field(nx,ny)
	
func gen_e_field(nx,ny):
	for i in range(ny):
		v_field.append([])								# Add a row in v_field matrix    
		for j in range(nx):
			var v = Sprite2D.new()							# Create a sprite object
			v.texture = load("res://arrow.png")					# Set sprite texture as file "res://arrow.png"
			v.scale = Vector2(1,1)*0.1						# Set sprite scale as 0.1
			v.global_position = Vector2(j/float(nx)*lx,i/float(ny)*ly)		# Set sprite position to a fraction of lx and ly 
			add_child(v)								# Add sprite to our scene
			
			v_field[i].append(v)							# Add sprite to v_field matrix
```


![Arrows](../pics/Arrows.png)

#### Modifying the Field

If you run your simulation now, you will see several arrows lined up, all pointing in the same direction. This is expected, as we haven't yet modified their direction or color. Fortunately, we can manipulate both easily in Godot.

```gdscript
func _ready():
	gen_e_field(nx,ny)
	update_vectors()

func update_vectors():
	for i in range(ny):
		for j in range(nx):
			var norm = sin(i/2.0)**2			# Set a variable of vector norm 
			var dir = Vector2(-i,j)				# Set a variable of vector direction
			v_field[i][j].modulate.a = norm			# Set alpha component of the color of our arrow as norm
			v_field[i][j].rotation = dir.angle()		# Set the rotation of our arrow as the angle of direction with x axis
```

What we just did was simply assign the transparency of our arrows using a function of their position, and we did the same for their direction. We will not use these function to our eletric field, but this method is general for any function we want.

![Modified Arrows](../pics/ModifiedArrows.png)


### Creating a Charge

One way to represent a point charge in our simulation is through a visual image.  
However, beyond its appearance, a point charge also has a **position**, a **velocity**, and, naturally, a **charge**.

To encapsulate both the **visual** and **mathematical** characteristics of the charge, we define it as an object.

In **Godot**, objects are defined as classes that can store:

- Variables
- Functions
- Other objects

```gdscript
class charge:
	extends Sprite2D
	var q = 1.0
	var m = 1.0
	var last_pos = Vector2(0,0)
	func _init(q,pos) -> void:
		self.q = q
		self.global_position = pos
		self.last_pos = pos
		if q < 0:
			texture = load("res://negativa.png")
			self.scale = Vector2(1,1)*0.05
		else:
			texture = load("res://positiva.png")
			self.scale = Vector2(1,1)*0.08
```

---

We define a class that:

- Inherits from `Sprite`
- Contains variables for `charge` called "q", and `last position` which will help us later.
- Receives initial values for `charge`, `last position` and `position` upon creation
- Sets its texture dynamically based on the charge value

This allows us to represent the charge both visually and physically within the simulation.

### Charge and Field Interactivity

#### Mouse Interaction

We have already created an object with all the necessary components to represent a charge. However, if we run the simulation, nothing happens — this is because we have not yet added the charge to the scene. Let’s do that now.

First of all, create a variable to store our charges.

```gdscript
var charges = []
```

```gdscript
func _input(event):
	if Input.is_action_just_released("mouse1"):
		var c = charge.new(1,get_global_mouse_position())
		add_child(c)
		charges.append(c)
	if Input.is_action_just_released("mouse2"):
		var c = charge.new(-1,get_global_mouse_position())
		add_child(c)
		charges.append(c)
```

#### Input Handling and Charge Instantiation

We used the `_input` function, which is automatically called whenever a mouse or keyboard event occurs, along with conditions that check for mouse button presses. In this way, we instantiate a charge with magnitude 1 at the mouse position and then add it to the scene.

#### Updating the Electric Field

Now that we can add a charge using mouse buttons, we want to observe how the electric field changes when new charges are introduced. To accomplish this, we define a function that computes the **total electric field** at a given point. We then use this function to update both the **transparency** and **direction** of the vector arrows representing the field.


```gdscript
func _physics_process(delta):
	update_vectors()

func get_e_field(pos):
	var k = 1
	var f = Vector2(0,0)
	for c in charges:
		f += k*c.q/(pos - c.global_position).length()**2*(pos - c.global_position)
	return f

func update_vectors():
	for i in range(ny):
		for j in range(nx):
			var norm = get_e_field(Vector2(j/float(nx)*lx, i/float(ny)*ly)).length()*50
			var dir = get_e_field(Vector2(j/float(nx)*lx, i/float(ny)*ly))
			v_field[i][j].modulate.a = norm
			v_field[i][j].rotation = dir.angle()
```
### Verlet Method

In our numerical model for solving the pendulum, [PENDULUM](../pasta/PENDULUM.html), we discussed Euler’s method — a simple but not very accurate approach. Now we introduce the **Verlet method**, which is slightly more sophisticated and offers a much faster decrease in error as the time step decreases. To derive the recursive equation for this method, we begin by expanding the position function in a Taylor series, one step forward and one step backward.

$$
x(t + \Delta t) = x(t) + x'(t)\Delta t + \frac{1}{2}x''(t) \Delta t^2 + \frac{1}{6}x^{(3)}(t) \Delta t^3 + \frac{1}{24}x^{(4)}(t) \Delta t^4 + \cdots
$$

$$
x(t - \Delta t) = x(t) - x'(t)\Delta t + \frac{1}{2}x''(t) \Delta t^2 - \frac{1}{6}x^{(3)}(t) \Delta t^3 + \frac{1}{24}x^{(4)}(t) \Delta t^4 - \cdots
$$

Adding both equations:

$$
x(t + \Delta t) + x(t - \Delta t) = 2x(t) + x''(t) \Delta t^2 + \frac{1}{12}x^{(4)}(t) \Delta t^4 + \cdots
$$

Rewriting:

$$
x(t + \Delta t) = 2x(t) - x(t - \Delta t) + x''(t) \Delta t^2 + \frac{1}{12}x^{(4)}(t) \Delta t^4 + \cdots
$$

Neglecting higher-order terms, we arrive at the final Verlet update formula:

$$
x(t + \Delta t) = 2x(t) - x(t - \Delta t) + a(t) \Delta t^2
$$

While Euler’s method has an error proportional to \\( \Delta t^2 \\), Verlet’s method has an error proportional to \\( \Delta t^4 \\), which leads to much faster convergence as \\( \Delta t \\) decreases. However, we now require knowledge of the previous position \\( x(t - \Delta t) \\), which must be stored at each step, and the acceleration \\( a(t) \\), which can be computed from the electric field.

## Motion of the Charges

To apply the **Verlet integration method** to our system, we first compute the electric field at the position of each charge. Multiplying this field by the charge yields the force, which, when divided by the particle mass, produces the acceleration.

This procedure is repeated for every charge at each frame. The parameter `delta` is used as the time step, and the particle positions are updated according to the Verlet scheme.

```gdscript
func _physics_process(delta):
	update_vectors()
	move(delta)

func get_accel(cp):
	var k = 10000.0                  # Coupling constant
	var f = Vector2(0,0)             # Total force
	for c in charges:                # Sum over all other charges
		if c != cp:
			f += k * c.q / ((cp.global_position - c.global_position).length() + 0.0001)**2 \
			     * (cp.global_position - c.global_position)
	var a = f * cp.q / cp.m          # Acceleration
	return a

func move(dt):
	for c in charges:
		var lp = c.global_position
		c.global_position = 2 * c.global_position - c.last_pos + get_accel(c) * dt**2
		c.last_pos = lp
```

After running the simulation and creating a few charges, one can already observe their motion and the continuous rearrangement of the electric field. However, the physical description is still incomplete.  
The missing ingredient is **magnetism**.

---

## Magnetic Field

# Magnetic Field of Point Charges

The magnetic field generated by point charges in arbitrary motion is not trivial. It is well-known that variations in magnetic flux can induce waves in both the electric and magnetic fields. In computational contexts, this typically requires solving differential equations over the entire space and time domain (we will address this later in a non-interactive way).  

At this stage, we adopt an **approximation**, treating the charges as **stationary currents**.

As a starting point, we can write the **2D Biot–Savart equation**:

$$
B(\vec{r}) = \frac{\mu_0}{2\pi} \int \frac{\hat{z} \cdot \big( \vec{J}(\vec{r'}) \times (\vec{r} - \vec{r'}) \big)}{|\vec{r} - \vec{r'}|^2}  ~d^2 r'
$$

---

## Current Density of a Point Charge

The current density of a point charge can be expressed as:

$$
\vec{J}(\vec{r}) = \frac{d\vec{I}}{dl} = \frac{d}{dl} \left( \frac{dq}{dt} \hat{u} \right)
$$

Using the **chain rule**, and assuming that the flow of charge along the current is constant and equal to \\(q\\):

$$
\frac{d}{dl} \left( \frac{dq}{dt} \ \hat{u} \right) 
= \frac{d}{dl} \left( \frac{dq}{ds} \frac{ds}{dt} \hat{u} \right) 
= \frac{d}{dl} (q \vec{v})
$$

Hence, the current density becomes:

$$
\vec{J}(\vec{r}) = q \delta(\vec{r}) \vec{v}
$$

---

## Magnetic Field of a Point Charge

Substituting this expression into the Biot–Savart equation:

$$
B(\vec{r}) = \frac{\mu_0}{2\pi} \int \frac{\hat{z} \cdot \Big( (q  \delta(\vec{r'})  \vec{v}) \times (\vec{r} - \vec{r'}) \Big)}{|\vec{r} - \vec{r'}|^2}  ~^2 r'
$$

This reduces to:

$$
B(\vec{r}) = \frac{\mu_0}{2\pi} \frac{q v \sin\theta}{|\eta|}
$$

where:  

{% raw %}

- \\( \eta = |\vec{r} - \vec{r'}| \\) is the distance between the point of observation and the point charge,  
- \\(\theta\\) is the angle between the velocity \\(\vec{v}\\) of the charge and \\(\eta\\).  

{% endraw %}
### Field Construction

To represent the magnetic field, we construct an image in which each pixel encodes the local field intensity through its color. This is achieved by adding a `Sprite2D` object to the main scene, which will display the dynamically updated field texture.

---

### Image Scaling and Spatial Mapping

Since the image consists of `nx × ny` pixels while representing a physical region of size `lx × ly`, the sprite must be properly scaled. The pixel grid is normalized and resized to match the physical dimensions of the simulation domain. Finally, the sprite is translated so that it is centered on the screen.

```gdscript
func _ready():
	gen_e_field(nx, ny)
	update_vectors()
	$Sprite2D.scale = Vector2(lx / float(nx), ly / float(ny))
	$Sprite2D.global_position = Vector2(lx / 2.0, ly / 2.0)
```

---

### Updating the Magnetic Field

A **scalar field** assigns a real number to every point in space, whereas an image is a discrete grid in which each point stores color information. Our goal is to connect these two representations.

For each pixel, we compute its corresponding physical position and evaluate the magnetic field at that point. The contribution from each charge is calculated using its velocity and the vector connecting the charge to the pixel. The resulting field value is then mapped to a color and written to the image.

```gdscript
func update_m_field():
	var img = Image.create(nx, ny, false, Image.FORMAT_RGBA8)
	for i in range(nx):
		for j in range(ny):
			var B = 0
			var pos = Vector2(i / float(nx) * lx, j / float(ny) * ly) + 0.5 * Vector2(1,1)
			for c in charges:
				var v = (c.global_position - c.last_pos) / get_physics_process_delta_time()
				var r = pos - c.global_position
				B += 0.25 * c.q * v.length() * sin(r.angle_to(v)) / r.length()
			
			if B > 0:
				img.set_pixel(i, j, Color(1, 1 - B, 1 - B))
			else:
				img.set_pixel(i, j, Color(1 + B, 1, 1 + B))

	var tex = ImageTexture.create_from_image(img)
	$Sprite2D.texture = tex
```

## Magnetic Waves

Suppose you have gone completely mad, and creating a set of charges moving according to the laws of 2D electromagnetism, with interactive electrostatic and magnetostatic fields, is no longer sufficient to quench your thirst for physics. In that case, I have some unfortunate news: the Godot engine is not optimized enough to solve systems far more complex than this (I have tried extensively). Indeed, up until now, we have only solved systems where the temporal evolution depends solely on the positions and velocities of the charges, and on the positions of each field point.  

Taking this to the next level would yield a system in which every point of the field depends on the positions and velocities of all charges, the value of the field at the previous timestep, and the values of the surrounding field points. This quickly becomes intractable to compute at every step and is generally too time-consuming to achieve reasonable precision in Godot.  

Our next step will be implemented in Python, which is far more efficient at handling computationally demanding problems. We will also employ several libraries to create animations of our system.  

The equation we aim to solve is the two-dimensional wave equation with a source term:

$$
\nabla^2 \mathbf{B} - \frac{1}{c^2} \frac{\partial^2 \mathbf{B}}{\partial t^2} = \mu_0 \nabla_{\perp} \cdot \vec{j}
$$

### Finite Difference Equation

A sufficiently well-behaved function can be expanded in the vicinity of a point using its Taylor series. Let us apply this expansion to both a forward and a backward step.

{% raw %}
$$
f(x + \Delta x) = f(x) + f'(x) \Delta x + \frac{f''(x) {\Delta x}^2}{2} + \frac{f^{(3)}(x) {\Delta x}^3}{6} + \mathcal{O}(\Delta x^2)
$$

$$
f(x - \Delta x) = f(x) - f'(x) \Delta x + \frac{f''(x) {\Delta x}^2}{2} - \frac{f^{(3)}(x) {\Delta x}^3}{6} + \mathcal{O}(\Delta x^4)
$$

By summing these two equations, we obtain:

$$
f(x + \Delta x) + f(x - \Delta x) = 2 f(x) + f''(x) {\Delta x}^2 + \mathcal{O}(\Delta x^4)
$$

Consequently, we can express the second derivative as:

$$
\frac{f(x + \Delta x) - 2 f(x) + f(x - \Delta x)}{{\Delta x}^2} = f''(x) + \mathcal{O}(\Delta x^2)
$$
{% endraw %}
This provides a formula for the second derivative of the function, with an error proportional to the square of the step size.

### Applying Finite Differences to the Magnetic Wave Equation

In this manner, we will apply finite differences to our magnetic wave equation.

{% raw %}

$$
\frac{B(x, y, t + \Delta t) - 2B(x, y, t) + B(x, y, t - \Delta t)}{\Delta t^2} = c^2 \left( \nabla^2 B(x, y, t) - \mu_0 \nabla_{\perp} \cdot \vec{j} \right)
$$

$$
B(x, y, t + \Delta t) = 2B(x, y, t) - B(x, y, t - \Delta t) + {(c \Delta t)}^2 \left( \nabla^2 B(x, y, t) - \mu_0 \nabla_{\perp} \cdot \vec{j} \right)
$$

Where:

$$
\nabla^2 B(x, y, t) = \frac{B(x + \Delta x, y, t) - 2B(x, y, t) + B(x - \Delta x, y, t)}{\Delta x^2} + \frac{B(x, y + \Delta y, t) - 2B(x, y, t) + B(x, y - \Delta y, t)}{\Delta y^2}
$$

This expression provides the magnetic field value at the next time step at each point, based solely on neighboring values and previous time steps. Solving this is the primary problem we must focus on. There remains only one unknown in this equation: the divergence of the current density.

Considering the current density as a Dirac delta function is a clever strategy in analytical calculations, as it faithfully describes a point charge. However, implementing a computationally infinite charge peak is rather complicated. Discontinuities are something we wish to avoid in any type of numerical computation.

What we have been doing so far to resolve numerical problems is to take limits, series, spaces, and variables, and truncate them to the extent we desire. Let us proceed with this approach!

We can rewrite the Dirac delta function as follows:

$$
\delta(\vec{r}) = \lim_{l_o \to 0} \frac{1}{2\pi l_o} e^{-(r_x^2 + r_y^2)/l_o}
$$

Observe that when we approach the charge, the exponential term tends to unity, and the expression diverges:

$$
\delta(\vec{r}\to 0) = \lim_{l_o \to 0} \frac{1}{2\pi l_o} = \infty
$$

Conversely, when we move away from the charge, so that \\(r_x^2\\) or \\(r_y^2\\) becomes larger than \\(l_o\\), the function rapidly vanishes:

$$
\delta(\vec{r}) = \lim_{l_o \to 0} \frac{1}{2\pi l_o} e^{-\infty} = 0
$$

Thus, by choosing a small but non-zero value of \(l_o\), we can perform our calculations using a Gaussian approximation. This provides a reasonable model for the current density and its curl:

$$
\vec{J}(\vec{r}) = \rho(\vec{r}) \vec{v} = q \frac{1}{2\pi l_o} e^{-(r_x^2 + r_y^2)/l_o} \vec{v}
$$

$$
\nabla \cdot \vec{J}(\vec{r}) = - q \frac{(-x v_y + y v_x)}{\pi l_o} e^{-(r_x^2 + r_y^2)/l_o}
$$
{% endraw %}
### Boundary Condition

The simplest boundary conditions, in which the field vanishes at the boundaries or is taken to be periodic, can be useful in many contexts. However, here we aim to observe waves generated by point charges. Allowing multiple waves to return from or emerge at the boundaries would significantly interfere with the visualization. For this reason, we employ an outgoing-wave boundary condition, known as the Sommerfeld boundary condition.

Consider a free-wave condition in one spatial dimension.

$$
\frac{\partial^2 u(x,t)}{\partial x^2}
-
\frac{1}{c^2}
\frac{\partial^2 u(x,t)}{\partial t^2}
=
0
$$

The general solution of this equation for a wave propagating to the right is of the form

$$
u(x,t) = f(x - ct)
$$

Thus,

$$
\frac{\partial u}{\partial x} = f'(x - ct)
$$

and

$$
\frac{\partial u}{\partial t} = -c f'(x - ct)
$$

Therefore,

$$
c\,\frac{\partial u}{\partial x}
+
\frac{\partial u}{\partial t}
=
c f'(x - ct) - c f'(x - ct)
=
0
$$

We can implement this condition when solving our equations numerically, so that at the boundaries the solution behaves as a free outgoing wave.


### Coding

#### Library

We will only use `numpy` and `matplotlib` for our simulation.

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
```

#### Variables

Analogously to our previous work in Godot, we create a grid for the magnetic field and lists for the properties of the charges.

```python
lx, ly = 20.0, 20.0          # Simulation box size
nx, ny = 70, 70              # Number of points in each direction
dx = lx / nx
dy = ly / ny

c = 10.0                     # Speed of light
dt = 5e-4
steps = 300                  # Number of simulation steps
interval_ms = 30             # Duration of each frame

B = np.zeros((ny, nx))       # Magnetic field matrice
B_old = np.zeros_like(B)     # Last magnetic field matrice

xs = np.array([              # Position of each charge
    [3.0, 5.0],
    [7.0, 5.0],
    [15,12]
])
vs = np.array([              # Speed of each charge
    [0.0,  2.0],
    [0.0, -2.0],
    [0,0]
])
qs = np.array([1.0, -1.0,1]) # Charge of each charge
xs_old = xs.copy()

k_coul = 100.0               # Proportionality constant
eps = 1e-2                   # Singularity break parameter
lo = 0.1
inv_lo = 1.0 / lo

X, Y = np.meshgrid(          # Create a grid
    np.linspace(0, lx, nx),
    np.linspace(0, ly, ny)
)
```

#### Solving

```python
def step():
    global B, B_old, xs, xs_old, vs                                             # Set global variables
    xs_new = xs.copy()                                                          # Copy the last positions

    for i in range(len(xs)):                                                    # Loop over all charges
        f = np.zeros(2)                                                         # Set a force vector
        for j in range(len(xs)):                                                # Loope over other charges
            if i != j:
                r = xs[i] - xs[j]                                               # Distance vector
                r2 = np.dot(r, r) + eps                                         # Quadratic distance plus positive number
                f += k_coul * qs[i] * qs[j] * r / r2                            # Coulomb force

        xs_new[i] = 2*xs[i] - xs_old[i] + f * dt**2                             # Verlet interaction
        vs[i] = (xs_new[i] - xs_old[i]) / (2*dt)                                # Centered difference for velocity

    xs_old[:] = xs                                                              # Update xs_old
    xs[:] = xs_new                                                              # Update xs

    lap = (                                                                     # Calculate laplacian of magnetic field
        (np.roll(B, 1, axis=0) - 2*B + np.roll(B, -1, axis=0)) / dy**2 +
        (np.roll(B, 1, axis=1) - 2*B + np.roll(B, -1, axis=1)) / dx**2
    )

    S = np.zeros_like(B)                                                        # Define a variable S for the source term
    for p in range(len(xs)):                                                    # Loop over all charges
        rx = X - xs[p, 0]                                                       # Displacement in x
        ry = Y - xs[p, 1]                                                       # Displacement in y
        r2 = rx**2 + ry**2                                                      # Quadratic distance
        vx, vy = vs[p]                                                          # Define vx and vy
        S += qs[p] * (-ry*vx + rx*vy) * np.exp(-r2 * inv_tau) * inv_tau         # Calculate source value

    B_new = 2*B - B_old + (c*dt)**2 * (lap - 0.2*S)                             # Calculate the new value of B

	# Implement boundary conditions
    cx = c * dt / dx
    cy = c * dt / dy

    B_new[:, 0]  = B[:, 0]  - cx * (B[:, 0]  - B[:, 1])
    B_new[:, -1] = B[:, -1] - cx * (B[:, -1] - B[:, -2])

    B_new[0, :]  = B[0, :]  - cy * (B[0, :]  - B[1, :])
    B_new[-1, :] = B[-1, :] - cy * (B[-1, :] - B[-2, :])

    B_old[:] = B
    B[:] = B_new


dt_frame = interval_ms / 1000.0                                                  # Time per frame
steps_per_frame = max(1, int(dt_frame / dt))                                     # Steps per frame

B_hist = np.zeros((steps, ny, nx))                                               # Save all fields along the time
xs_hist = np.zeros((steps, len(xs), 2))                                          # Save all positions along the time

for n in range(steps):                                                           # Loop over all frames
    for _ in range(steps_per_frame):                                             # Loop over all steps in each frame
        step()                                                                   # Solve one step
    B_hist[n] = B                                                                # Save the field
    xs_hist[n] = xs                                                              # Save the position

```

#### Create animation

```python
from matplotlib.colors import LinearSegmentedColormap                            # Import library

fig, ax = plt.subplots(figsize=(6,6))                                            # Create figure


cmap_rwg = LinearSegmentedColormap.from_list(                                    # Create colormap
    "RedWhiteGreen",
    ["lime", "white", "red"]
)

im = ax.imshow(                                                                  # Create first frame field picture
    B_hist[0],
    extent=[0, lx, 0, ly],
    origin="lower",
    cmap=cmap_rwg,
    vmin=-0.1,
    vmax=0.1
)

scat = ax.scatter(                                                               # Create colorful points
    xs_hist[0,:,0],
    xs_hist[0,:,1],
    c=["red", "blue","red"],
    s=80
)

ax.set_xticks([])                                                                # Take off ticks
ax.set_yticks([])

cbar = fig.colorbar(im, ax=ax,shrink=0.75)                                       # Set colorbar
cbar.set_label("Scalar Magnetic Field", fontsize=16)                             # Set label

cbar.set_ticks([])                                                               # Take off ticks

def update(frame):                                                               # Update animation function
    im.set_data(B_hist[frame])
    scat.set_offsets(xs_hist[frame])
    return im, scat

ani = FuncAnimation(                                                            # Create animation
    fig,
    update,
    frames=steps,
    interval=interval_ms
)

ani.save(                                                                       # Save animation
    "wave.gif",
    writer="ffmpeg",
    fps=500 // interval_ms,
    dpi=150
)

```

<div style="display: flex; align-items: center; gap: 50px;">

  <img src="./pics/onda_leve.gif" width="600">

</div>
