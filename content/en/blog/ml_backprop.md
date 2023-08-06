---
author: "Benji Peng"
title: "Backpropagation - Matrix Form"
description: "An attempt to explore the intricate interplay among diverse industries, from infrastructure to consumer services"
tags: ["branding", "profile", "fundamentals"]
date: 2023-08-05
thumbnail: https://raw.githubusercontent.com/benjipeng/assets/main/rc/blog/ml/llm/gradient.jpg
math: true
toc: true
---

Backpropagation stands as a fundamental algorithm that plays a pivotal role in training neural networks, working in tandem with an optimization routine like gradient descent. The crux of gradient descent lies in its ability to minimize the loss function by accessing the gradients with respect to all the network's weights and performing weight updates accordingly.

> A layer of a NN can be considered as an Affine Transformation (a linear mapping method that preserves points, straight lines, and planes) followed by application of a non linear function.

- A `vector` is received as `input`.
- The `vector` is multiplied with a matrix to produce an `output`.
- A `bias` vector may be added to the *output*.
- Pass the result through an `activation function` such as `sigmoid`.
- $\small Input = x, \  Output = f(Wx+b)$

***Forward Propagation*** equations are as follows:

$$
\begin{matrix}
  \textnormal{\footnotesize{Input}}=x_0 \\\
  \footnotesize{Hidden\ Layer}_1 \ \textnormal{\footnotesize{output}}= x_1 = f(W_1x_0) \\\
  \footnotesize{Hidden\ Layer}_2 \ \textnormal{\footnotesize{output}}= x_2 =f(W_2x_1) \\\
  \textnormal{\footnotesize{Output}} = x_3 = \footnotesize{f_3(W_3x_2)} \\\
\end{matrix}
$$

## Loss Function

***Stochastic gradient descent*** uses a *single* instance to perform weight updates, whereas the ***Batch gradient descent*** uses a *batch* of data ($t$ is the ground truth).

- *Stochastic* loss function: $E=1/2{\Vert z - t \Vert}^2_2$
- *Batch* loss function: $E=1/2\sum_ {\isin Batch}{\Vert z_i - t_i \Vert}^2_2$

Given an input $x_0$, $x_3$ can be represented by $x_0$ and $W_1, W_2, W_3$. $W_1, W_2, W_3$ are the tunable parameters in this case.

$$w = w - \alpha_w \frac{\partial E}{\partial w} \ \ \textnormal{\footnotesize{for all weights w}}$$

$\alpha_w$ is called the ***learning rate***, a scalar for this particular weight.

### Chain Rule

For $z = f(y), \ y=g(x)$, there is $\frac{\partial z}{\partial x} = \frac{\partial z}{\partial y} \frac{\partial y}{\partial x}$.

Backpropagation equations can be derived by applying the **chain rule** on $E=1/2{\Vert x_3 - t \Vert}^2_2, \ x_3 = \small{f_3(W_3x_2)}$.

$$
\frac{\partial E}{\partial W_3} = (x_3 -t) \frac{\partial x_3}{\partial W_3}
$$
