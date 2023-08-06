---
author: "Benji Peng"
title: "Intro to Matrix Calculus"
description: "An attempt to explore the intricate interplay among diverse industries, from infrastructure to consumer services"
tags: ["branding", "profile", "fundamentals"]
date: 2023-08-05
thumbnail: https://raw.githubusercontent.com/benjipeng/assets/main/rc/blog/ml/math/matrix.jpg
math: true
toc: true
---

## Derivative of Vector Functions

Let $x$ and $y$ be vectors of orders $n$ and $m$:

$$
\mathbf{x} =
\begin{bmatrix}
  x_1 \\\
  x_2 \\\
  \vdots \\\
  x_n
\end{bmatrix}, \space
\mathbf{y} =
\begin{bmatrix}
  y_1 \\\
  y_2 \\\
  \vdots \\\
  y_m
\end{bmatrix}
$$

Each component $y_i$ may be a function of all the $x_j$ ($\mathbf{y}$ is a
function of $\mathbf{x}$), or $\mathbf{y} = \mathbf{y(x)}$.

### Derivative of a Vector with Respect to another Vector

By definition, the derivative of the vector **y** with respect to vector **x** is the n × m matrix.

$$
\frac{\partial \textbf{y}}{\partial \textbf{x}}\overset{\textnormal{def}}{=}\overbrace{
\begin{bmatrix}
  \frac{\partial y_1}{\partial x_1} & \frac{\partial y_2}{\partial x_1} & \dots & \frac{\partial y_m}{\partial x_1} \\\
  \frac{\partial y_1}{\partial x_2} & \frac{\partial y_2}{\partial x_2} & \dots & \frac{\partial y_m}{\partial x_2} \\\
  \vdots & \vdots & \ddots & \vdots \\\
  \frac{\partial y_1}{\partial x_n} & \frac{\partial y_2}{\partial x_n} & \dots & \frac{\partial y_m}{\partial x_n}
\end{bmatrix}}^{\textnormal{n}\times\textnormal{m}}
$$

### Derivative of a Scaler with Respect to another Vector

$$
\frac{\partial\mathbf{y}}{\partial x}=
\begin{bmatrix}
  \tfrac{\partial y_1}{\partial x} & \tfrac{\partial y_2}{\partial x} &
  \dots &
  \tfrac{\partial y_m}{\partial x}
\end{bmatrix}
$$

$$
\frac{\partial y}{\partial \mathbf{x}}=
\begin{bmatrix}
  \tfrac{\partial y}{\partial x_1} \\\ \tfrac{\partial y}{\partial x_2} \\\
  \vdots \\\
  \tfrac{\partial y}{\partial x_n}
\end{bmatrix}
$$

$x,y$ are scalers, while $\textbf{x}, \textbf{y}$ are vectors with length $\textnormal{n}$ and $\textnormal{m}$.

#### Relationships

$$
\mathbf{y}=A\mathbf{x}=\underbrace{
\begin{bmatrix}
a_{11} & a_{12} & \dots & a_{1n} \\\
a_{21} & a_{22} & \dots & a_{2n} \\\
\vdots & \vdots & \ddots & \vdots \\\
a_{m1} & a_{m2} & \dots & a_{mn}
\end{bmatrix}}_ {\textnormal{m}\times\textnormal{n}}\overbrace{\begin{bmatrix}
x_{1} \\\
x_{2} \\\
\vdots \\\
x_{n}
\end{bmatrix}}^{\textnormal{n} \times 1}
$$

$$
=\begin{bmatrix}
  a_{11}x_1 + a_{12}x_2 + \dots + a_{1n}x_n \\\
  a_{21}x_1 + a_{22}x_2 + \dots + a_{2n}x_n \\\
  \vdots \\\
  a_{m1}x_1 + a_{m2}x_2 + \dots + a_{mn}x_n
\end{bmatrix}=\underbrace{\begin{bmatrix}
  \sum_ {l=1}^n a_{1l}x_l = y_1 \\\
  \sum_ {l=1}^n a_{2l}x_l = y_2 \\\
  \vdots \\\
  \sum_ {l=1}^n a_{ml}x_l = y_m
\end{bmatrix}}_ {\textnormal{m} \times 1}
$$

$y_m$ in $\mathbf{y}$ can therefore be represented as $y_m = \displaystyle\sum_{l=0}^n a_{ml} \cdot x_l$. Based on how $\frac{\partial \textbf{y}}{\partial \textbf{x}}$ is defined, $\frac{\partial y_m}{\partial x_1}$, for example, becomes $a_{m1}$. Therefore $\frac{\partial \textbf{y}}{\partial \textbf{x}}=A^{T}$.
