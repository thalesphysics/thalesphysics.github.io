---
title: "Physics Simulators"
layout: default
---

<style>
.site-header {
  display: none !important;
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

# About this project

This website gathers a series of projects developed during my undergraduate studies, focusing on the **computational simulation of physical systems**. The main goal is to make available:

- Interactive simulators that can be run directly in the browser  
- Theoretical explanations of the physical models involved  
- Well-documented and reusable source code

The simulations were implemented using **Python** and the **Godot Engine**, aiming to explore both the physical behavior and the computational visualization of the systems modeled.

> Note: This content is personal and independent, and has no formal affiliation with any academic institution or research program.

---

# Available simulators

 

<div style="display: flex; align-items: center; gap: 50px;">

  <img src="./pics/pendulum.png" width="300">

  <div style="font-size: 1.2em;">
    <strong><a href="./pasta/PENDULUM.html">Damped Pendulum</a></strong><br>
    Numerical solution of the damped pendulum's equation of motion using the Euler method.
    In addition, user interaction was implemented, allowing the pendulum to be moved with the mouse.
    System parameters such as pendulum length and air viscosity can also be adjusted freely.
  </div>

</div>

<div style="display: flex; align-items: center; gap: 50px;">

  <img src="./pics/charges.png" width="300">

  <div style="font-size: 1.2em;">
    <strong><a href="./pasta/CHARGE.html">2D Electromagnetism</a></strong><br>
    It is possible to implement the laws of electromagnetism in three dimensions numerically within a simulation. However, this can be quite complex and computationally expensive. As an alternative, we could attempt to constrain the simulation to two dimensions, at the cost of discarding some of Maxwell's equations — which would be undesirable.
    
    In this project, we develop the dynamics of charged particles iteratively, implementing a theoretical model of electromagnetism in a 2D world.

  </div>

</div>


## [Coupled Mass-Spring System](./simulators/coupled_springs.html)  
## [Oscillating Rings](./simulators/oscillating_rings.html)  
## [Dancing Flames](./simulators/dancing_flames.html)  
## [Projectile with Spring](./simulators/projectile_spring.html)  
## [Inclined Plane with Ball](./simulators/inclined_plane.html)

Each simulation is accompanied by a brief theoretical description and the corresponding implementation. In some cases, the source code is available for download or direct access.

---

# About the Author

<div style="display: flex; align-items: flex-start; gap: 40px;">

  <!-- Image on the left -->
  <div>
    <img src="./pics/5087243562612624401.jpg" style="width: 400px; max-width: 100%; border-radius: 8px;" alt="Author photo">
  </div>

  <!-- Text on the right -->
  <div style="max-width: 600px; line-height: 1.6;">

    <p>My name is <strong>Thales</strong>, and I am an undergraduate student of Physics at the <strong>Federal University of ABC (UFABC)</strong>, located in the ABC metropolitan region of Brazil.</p>

    <p>I have worked with simulations related to:</p>
    <ul>
      <li>the <strong>maximum hydrogen adsorption capacity of materials</strong>;</li>
      <li>the <strong>prediction of mechanical properties</strong>;</li>
      <li>the <strong>prediction of electrical properties</strong>;</li>
      <li>the <strong>prediction of catalytic capacities for hydrogen production</strong>.</li>
    </ul>

    <p>In general, I employed computational methods based on <strong>classical atomistic dynamics</strong> and <strong>quantum mechanics</strong>.</p>

    <p>Furthermore, I have participated in <strong>physics competitions</strong> focused on solving simple, open-ended physics problems, both <strong>theoretically</strong> and <strong>experimentally</strong>.</p>

    <p>For feedback, suggestions, or collaboration inquiries, feel free to reach out via <a href="https://github.com/thalesphysics">GitHub</a> or email at <strong>thales.machado.fernandes@gmail.com</strong>.</p>

  </div>

</div>


---

<center>
This site is powered by [GitHub Pages](https://pages.github.com)  
Equations are rendered using [MathJax](https://www.mathjax.org/)
</center>
