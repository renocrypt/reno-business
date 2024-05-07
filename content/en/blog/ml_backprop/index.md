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

## A Simple Overview

A simple Neuron Network (NN) can be represented by the following:

$$
\colorbox{blue}{$\vec{\text{x}}$} \Rightarrow
\underbrace{\colorbox{green}{$\text{NN}$}}_ {\theta} \Rightarrow
\colorbox{brown}{$\mathbf{\vec{y}}$} \xLeftrightarrow{C^n}
\colorbox{brown}{$\mathbf{\vec{\hat{y}}}$}
$$

$\vec{\text{y}}$ represents the output from the NN and $\mathbf{\vec{\hat{y}}}$ is the _ground truth_. A more detailed (yet very abstract) relationship between the input and output _via_ the NN is given in the _GoAT_ plot below.

```goat
                                                       +------+
+-----+                                       w_3      |      |
|     |  w_1     .-----.          +---+--------------->|  z'  +----> a' ...
| x_1 +--------->|     |   = z    |   |                |      |
|     |          | sum +--------->| a |                +------+
+-----+      +-->|     |          |   |
             |   +-+---.          +---+----+
+-----+      |     |                       |           +------+
|     |      |   +-+-+                     |           |      |
| x_2 +------+   |   |                     +---------->|  z"  +----> a' ...
|     |  w_2     | b |                        w_4      |      |
+-----+          |   |                                 +------+
                 +---+
```

`a` represents the activation function, which has an input of z (for a sigmoid function, $a=\sigma(z)$). At the level of an individual neuron, $z = w_1x_1 + w_2x_2 + b$.

The loss function $L(\theta)$ in this case is:

$$
L(\theta) = \displaystyle\sum^N_ {n=1}C^n(\theta)
\textnormal{, } \dfrac{\partial L(\theta)}{\partial w}=
\displaystyle\sum^N_ {n=1}\dfrac{\partial C^n(\theta)}{\partial w}
$$

The process of getting $\frac{\partial L}{\partial w}$ is equivalent to obtaining all $\frac{\partial C}{\partial w}$s.

### Using Chain Rule

$C$ is can be expressed as a function of $z$, while $z$ is a function of $w$s. Based on chain rule, $\frac{\partial C}{\partial w}$ can be obtained from $\frac{\partial C}{\partial z} \cdot \frac{\partial z}{\partial w}$.

- **Forward pass**: Compute $\frac{\partial z}{\partial w}$ for all parameters.
- **Backward pass**: Compute $\frac{\partial C}{\partial z}$ for all activation function inputs z.

### Forward Pass

The forward pass is very straightforward.

$\frac{\partial z}{\partial w_1}=x_1$ and $\frac{\partial z}{\partial w_2}=x_2$, which is the value of inputs connected by the weights.

### Backward Pass

Now we have $\frac{\partial C}{\partial z}$ left to figure out. Due to the relationship between $a$ and $z$, $\frac{\partial C}{\partial z} = \frac{\partial C}{\partial a}\frac{\partial a}{\partial z}$.

$\frac{\partial a}{\partial z}$ is indeed $\sigma'(z)$, which is an constant.

$\frac{\partial C}{\partial a}$ is more complicated. According to the `GoAT` plot, $C$ can be expressed in terms of $Z$s. $\frac{\partial C}{\partial a} = \frac{\partial z'}{\partial a}\frac{\partial C}{\partial z'} + \frac{\partial z''}{\partial a}\frac{\partial C}{\partial z''}$, $\frac{\partial C}{\partial a} = w_3\frac{\partial C}{\partial z'} + w_4\frac{\partial C}{\partial z''}$.

$\frac{\partial C}{\partial z}$ is therefore $\sigma'(z) [w_3\frac{\partial C}{\partial z'} + w_4\frac{\partial C}{\partial z''}]$

## Matrix Representation

Here are some notes on

- A `vector` is received as `input`.
- The `vector` is multiplied with a matrix to produce an `output`.
- A `bias` vector may be added to the _output_.
- Pass the result through an `activation function` such as `sigmoid`.
- $\small Input = x, \  Output = f(Wx+b)$

**_Forward Propagation_** equations are as follows:

$$
\begin{matrix}
  \textnormal{\footnotesize{Input}}=\vec{\text{x}}_0 \\\
  \footnotesize{Hidden\ Layer}_1 \ \textnormal{\footnotesize{output}}= \vec{\text{x}}_1 = f_1(W_1\vec{\text{x}}_0) \\\
  \footnotesize{Hidden\ Layer}_2 \ \textnormal{\footnotesize{output}}= \vec{\text{x}}_2 =f_2ds(W_2\vec{\text{x}}_1) \\\
  \textnormal{\footnotesize{Output}} = \vec{\text{x}}_3 = \footnotesize{f_3(W_3\vec{\text{x}}_2)} \\\
\end{matrix}
$$

## Loss Function

**_Stochastic gradient descent_** uses a _single_ instance to perform weight updates, whereas the **_Batch gradient descent_** uses a _batch_ of data ($y$ is the ground truth).

- _Stochastic_ loss function: $L=1/2{\Vert \hat{y} - y \Vert}^2_2$
- _Batch_ loss function: $L=1/2\sum_ {\isin Batch}{\Vert \hat{y}_i - y_i \Vert}^2_2$

Given an input $x_0$, $x_3$ can be represented by $x_0$ and $W_1, W_2, W_3$. Matrices $W$s represent tunable parameters in the process.

$$w = w - \alpha_w \frac{\partial L}{\partial w} \ \ \textnormal{\footnotesize{for all weights w}}$$

$\alpha_w$ is called the **_learning rate_**, a scalar for this particular weight.

### Chain Rule

For $z = f(y), \ y=g(x)$, there is $\frac{\partial z}{\partial x} = \frac{\partial z}{\partial y} \frac{\partial y}{\partial x}$.

Backpropagation equations can be derived by applying the **chain rule** on $\hat{\vec{y}} = \vec{\text{x}}_3 = \small{f_3(W_3\vec{\text{x}}_2)}$ and $L=1/2{\Vert \vec{\text{x}}_3 - \vec{\text{y}} \Vert}^2_2$.

$$
\frac{\partial L}{\partial W_3} = \Vert \vec{\text{x}}_3 -\vec{\text{y}} \Vert \frac{\partial \vec{\text{x}}_3}{\partial W_3}=
\Vert \vec{\text{x}}_3 -\vec{\text{y}} \Vert \frac{\partial (W_3\vec{\text{x}}_2+\vec{\text{b}}_3)}{\partial W_3}
$$

$$
\frac{\partial E}{\partial W_3} = \Vert \vec{\text{x}}_3 -\vec{\text{y}} \Vert \frac{\partial \vec{\text{x}}_3}{\partial W_3}=
\Vert \vec{\text{x}}_3 -\vec{\text{y}} \Vert \frac{\partial (W_3\vec{\text{x}}_2+\vec{\text{b}}_3)}{\partial W_3}
$$
