---
title: "Physics Simulators"
layout: default
---

<!-- ============================= -->
<!-- Global settings & responsiveness -->
<!-- ============================= -->

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
/* Hide default header */
.site-header {
  display: none !important;
}

/* Links */
a {
  color: #59b390;
  text-decoration: none;
}
a:hover {
  color: #006400;
  text-decoration: underline;
}

/* ============================= */
/* Simulator blocks */
/* ============================= */

.sim-block {
  display: flex;
  align-items: center;
  gap: 40px;
  margin: 40px 0;
}

.sim-img {
  width: 300px;
  max-width: 100%;
  border-radius: 8px;
}

.sim-text {
  font-size: 1.1em;
  line-height: 1.6;
}

/* ============================= */
/* Author section */
/* ============================= */

.author-block {
  display: flex;
  align-items: flex-start;
  gap: 40px;
}

.author-img {
  width: 400px;
  max-width: 100%;
  border-radius: 8px;
}

/* ============================= */
/* Mobile layout */
/* ============================= */

@media (max-width: 768px) {

  .sim-block,
  .author-block {
    flex-direction: column;
    text-align: center;
  }

  .sim-text {
    font-size: 1em;
  }

  .sim-img,
  .author-img {
    max-width: 320px;
  }
}
</style>

<!-- ============================= -->
<!-- MathJax -->
<!-- ============================= -->

<script async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

# About this project

This website gathers a collection of projects developed during my undergraduate studies, with a focus on the **computational simulation of physical systems**. The main objectives are:

- Interactive simulators that run directly in the browser  
- Clear theoretical explanations of the underlying physical models  
- Well-documented and reusable source code  

The simulations were implemented using **Python** and the **Godot Engine**, exploring both the physical behavior of the systems and their computational visualization.

> **Note:** This content is personal and independent, with no formal affiliation to any academic institution or research program.

---

# Available simulators

<div class="sim-block">
  <img src="./pics/pendulum.png" class="sim-img">
  <div class="sim-text">
    <strong><a href="./pasta/PENDULUM.html">Damped Pendulum</a></strong><br>
    Numerical solution of the damped pendulum equation of motion using the Euler method.
    User interaction allows the pendulum to be displaced with the mouse, and system parameters
    such as pendulum length and air viscosity can be freely adjusted.
  </div>
</div>

<div class="sim-block">
  <img src="./pics/charges.png" class="sim-img">
  <div class="sim-text">
    <strong><a href="./pasta/CHARGE.html">2D Electromagnetism</a></strong><br>
    While a full three-dimensional numerical implementation of electromagnetism is computationally expensive,
    restricting the dynamics to two dimensions offers an instructive alternative.
    In this project, we develop the iterative dynamics of charged particles using a consistent
    theoretical model of electromagnetism in a 2D world.
  </div>
</div>

<div class="sim-block">
  <img src="./pics/springs.png" class="sim-img">
  <div class="sim-text">
    <strong><a href="./pasta/SPRING.html">Coupled Mass–Spring System</a></strong><br>
    Interacting many-body systems coupled by restoring forces are ubiquitous in physics.
    Their analytical treatment often requires solving large eigenvalue problems.
    Here, we employ numerical methods using the Godot Engine in conjunction with Python
    to study the system dynamics.
  </div>
</div>

## Additional simulations

- [Oscillating Rings](./simulators/oscillating_rings.html)  
- [Dancing Flames](./simulators/dancing_flames.html)  
- [Projectile with Spring](./simulators/projectile_spring.html)  
- [Inclined Plane with Ball](./simulators/inclined_plane.html)
- [Machine Learning](./stonles/index.html)

Each simulation is accompanied by a concise theoretical discussion and its corresponding implementation.
In some cases, the source code is available for direct access or download.

---

# About the Author

<div class="author-block">

  <img src="./pics/5087243562612624401.jpg"
       class="author-img"
       alt="Author photo">

  <div style="max-width: 600px; line-height: 1.6;">
    <p>
      My name is <strong>Thales</strong>, and I am an undergraduate Physics student at the
      <strong>Federal University of ABC (UFABC)</strong>, located in the ABC metropolitan region of Brazil.
    </p>

    <p>I have worked with simulations related to:</p>
    <ul>
      <li><strong>Maximum hydrogen adsorption capacity of materials</strong></li>
      <li><strong>Mechanical properties</strong></li>
      <li><strong>Electrical properties</strong></li>
      <li><strong>Catalytic capacities for hydrogen production</strong></li>
    </ul>

    <p>
      In general, I employed computational methods based on
      <strong>classical atomistic dynamics</strong> and
      <strong>quantum mechanics</strong>, and occasionally
      <strong>machine learning</strong>.
    </p>

    <p>
      I have also participated in <strong>physics competitions</strong> focused on solving
      simple, open-ended physics problems, both <strong>theoretically</strong> and
      <strong>experimentally</strong>.
    </p>

    <p>
      For feedback, suggestions, or collaboration inquiries, feel free to reach out via
      <a href="https://github.com/thalesphysics">GitHub</a> or email at
      <strong>thales.machado.fernandes@gmail.com</strong>.
    </p>
  </div>

</div>

---

<p style="text-align: center; font-size: 0.9em;">
  This site is powered by
  <a href="https://pages.github.com">GitHub Pages</a><br>
  Equations are rendered using
  <a href="https://www.mathjax.org/">MathJax</a>
</p>
