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

Um sistema importante na física é o sistema massa-mola. Esse sistema pode ser generalizado para o caso em que todas essas massas e molas estão acopladas entre sí. Nesse tópico, será discutido a importância física, e um modelo teórico para esse sistema. Além disso, as soluções serão obtidas numéricamente utilizando Python em conjunto com Godot.

## Why Masses and Springs?

Uma massa e uma mola grudada em uma parede, por que estudar um sistema tão simples e mundano? A verdade é muito mais profunda do que isso. Qualquer sistema com equilíbrios estáveis pode ser reduzido a um conjunto de massas e molas.

Imaginemos, primeiramente, um sistema unidimensional, como por exemplo um vale com uma bola. Se pudessemos supor que a força que age na bola é suficientemente suave, então sua energia potêncial em função de sua posição horizontal pode ser descrita por uma série de taylor:

$$
U(x) = U(x_o) + U'(x_o)(x-x_o) + \frac{U''(x_o)(x-x_o)^2}{2} + ...
$$

É evidente que o fundo do vale é um mínimo de potêncial, já que a energia potencial gravitacional é proporcional a altura, e a altura minima é o fundo do vale. Isso indica dois fatos importantes:

$$
U'(x_o) = 0
$$
$$
U''(x_o) > 0
$$

Além disso, é possível tomar como referência o potêncial do fundo do posso, de modo que temos ainda

$$
U(x_o) = 0
$$

Todos esses resultados nos levam a uma simplificação da expansão:

$$
U(x) = \frac{U''(x_o)(x-x_o)^2}{2} + \frac{U'''(x_o)(x-x_o)^3}{6} +...
$$

Em muitos casos não estamos tão interessados no movimento completo dos objetos, mas apenas em como esse movimento ocorre ao redor desses pontos de mínimo potêncial. Isso nos leva a uma simplificação drástica se assumirmos que os deslocamentos são pequenos. Definindo

$$
U''(x_o) = k
$$
$$
x - x_o = \Delta x
$$

Assim

$$
U(x) = \frac{k\Delta^2}{2} + math{O}(\Delta x^3)
$$
