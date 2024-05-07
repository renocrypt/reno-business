---
author: "Benji Peng"
title: "A Gentle Intro to Transformers"
description: "An attempt to explore the intricate interplay among diverse industries, from infrastructure to consumer services"
tags: ["branding", "profile", "fundamentals"]
date: 2023-08-02
thumbnail: https://raw.githubusercontent.com/benjipeng/assets/main/rc/blog/ml/llm/transformer.jpg
math: true
toc: true
---

> Transformers are a type of model architecture used in machine learning, particularly in the field of natural language processing (NLP). They were introduced in a paper titled **'Attention is All You Need'** by `Vaswani et al.` in **2017**. The main innovation of the Transformer model is the self-attention mechanism, which allows the model to weigh the importance of words in an input sequence when making predictions.

A *typical* Transformer model contains:

1. **Input Embedding Layer**: This is where the input data (usually text data) is converted into a numerical form that the model can understand. Each word (or token) in the input data is represented as a high-dimensional vector in this embedding space.

2. **Positional Encoding**: This is a step unique to Transformers. Because Transformers do not inherently understand the order of the data (unlike RNNs or LSTMs), positional encoding is added to give the model information about the position of the words in the input sequence.

3. **Encoder**: The encoder takes the embedded input data and processes it using self-attention and feed-forward neural networks. The Transformer model typically has multiple layers of these encoders.

    - `Self-Attention`: This mechanism utilizes `Scaled Dot-Product Attention`, which `allows the model to weigh the importance of words in the input sequence. For each word, it determines a score indicating how much attention should be paid to each other word when predicting the next word. $\quad\mathrm{Attention}(Q,K,V)$=$\textrm{softmax}\(\frac{QK^T}{\sqrt{d_k}}\)V$

    - `Feed-Forward Neural Networks`: These are standard fully connected neural networks that follow the self-attention mechanism in each encoder layer.

4. **Decoder**: The decoder also consists of layers of self-attention and feed-forward neural networks, but with an additional layer of cross-attention that pays attention to the output of the encoder. The decoder generates the output sequence.

5. **Output Linear Layer and Softmax**: The output from the decoder is passed through a linear layer followed by a softmax function to generate the final output probabilities for each possible next word in the sequence.

## Mathematical Representation

### Self-attention Mechanism

> Calculate `Query`, `Key`, and `Value` **vectors**: For each word in the input sequence, we calculate the Query, Key, and Value vectors. These are obtained by multiplying the embedding of the word by the learned weight matrices ($W_Q$, $W_K$, $W_V$).

> The `self-attention mechanism` in the Transformer model calculates the **attention score** for *each* word in the `input sequence` with respect to `every other word`. This is done using the `Query`($Q$, $Q \in \Reals^{m \times d_k}$), `Key` ($K$, $K \in \Reals^{n \times d_k}$), and `Value` ($V$, $V \in \Reals^{n \times d_v}$) vectors that are learned during training.

Dive in, we have the following:

$$
Q = \begin{pmatrix}
   − & \vec{q}_0 & − \\\
   − & \vec{q}_1 & − \\\
   & \vdots & \\\
   − & \vec{q}_m & −
\end{pmatrix}; K^T = \begin{pmatrix}
| & | & & | \\\
\vec{k}_0 & \vec{k}_1 & \dots & \vec{k}_n \\\
| & | & & |
\end{pmatrix}
$$

$$
Q \times K^{T} = \begin{pmatrix}
\vec{q}_0 \cdot \vec{k}_0 & \vec{q}_0 \cdot \vec{k}_1 & \dots & \vec{q}_0 \cdot \vec{k}_n \\\
\vec{q}_1 \cdot \vec{k}_0 & \vec{q}_1 \cdot \vec{k}_1 & \dots & \vec{q}_1 \cdot \vec{k}_n \\\
\vdots & \vdots & \ddots & \vdots\\\
\vec{q}_m \cdot \vec{k}_0 & \vec{q}_m \cdot \vec{k}_1 & \dots & \vec{q}_m \cdot \vec{k}_n 
\end{pmatrix}
$$

After this, the equation $\textnormal{softmax}(\frac{Q \times K^{T}}{\sqrt{d_k}})$ is `equal` to (dividing *each* element by $\sqrt{d_k}$ and perform $\textnormal{softmax}$ function ***PER ROW***).

$$\textnormal{softmax}(\frac{Q \times K^{T}}{\sqrt{d_k}})=
\begin{pmatrix}
  \textnormal{softmax}(\frac{1}{\sqrt{d_k}}\lang \vec{q}_0 \cdot \vec{k}_0, \vec{q}_0 \cdot \vec{k}_1, \dots ,\vec{q}_0 \cdot \vec{k}_n \rang) \\\
  \textnormal{softmax}(\frac{1}{\sqrt{d_k}}\lang \vec{q}_1 \cdot \vec{k}_0, \vec{q}_1 \cdot \vec{k}_1, \dots ,\vec{q}_1 \cdot \vec{k}_n \rang) \\\
  \vdots \\\
  \textnormal{softmax}(\frac{1}{\sqrt{d_k}}\lang \vec{q}_m \cdot \vec{k}_0, \vec{q}_m \cdot \vec{k}_1, \dots ,\vec{q}_m \cdot \vec{k}_n \rang)
\end{pmatrix}$$

$$
=\begin{pmatrix}
  s_{0,0} & s_{0,1} & \dots & s_{0,n} \\\
  s_{1,0} & s_{1,1} & \dots & s_{1,n} \\\
  \vdots & \vdots & \ddots & \vdots\\\
  s_{m,0} & s_{m,1} & \dots & s_{m,n}
\end{pmatrix} = S
$$

For each row $i$, there is $\displaystyle\sum_{j=1}^n s_{i,j}= 1$.

Then $\textnormal{softmax}(\frac{Q \times K^{T}}{\sqrt{d_k}})V$=
$S\begin{pmatrix}
    \vec{v}_0 \\\
    \vec{v}_1 \\\
   \vdots\\\
    \vec{v}_n
\end{pmatrix} = $

$\begin{pmatrix}
  \sum^n_ {i=0} s_{0,i} \vec{v}_ i  \\\
  \sum^n_ {i=0} s_{1,i} \vec{v}_ i  \\\
  \vdots \\\
  \sum^n_{i=0} s_{m,i} \vec{v}_ i
\end{pmatrix}=Z$

This produces the `Attention Score Vector` ($Z$).

#### Additional Notes

**NotePad**: **The default orientation of vectors is column vectors in *Computer Science***. Now let's dive in, $S$ should be a $(m \times n)$ matrix, $V$, therefore *MUST* be $(n *n)$, therefore, $\vec{v}*0$ can be represented as $\lang v_{0,0}, v_{0,1}, v_{0,2}, \dots ,v_{0,n} \rang$. Keep in mind that the $AV$ operation is ***NOT*** cross-product, it's indeed ***dot product*** (we use $A$ to denote the Query-Key).

The first step for `attention` is to do a matrix multiply between the Query (Q) matrix and a transpose of the Key (K) matrix (going through each word). *i.e.* Each column in the fourth row of $A$ corresponds to a dot product between the fourth word in `Query` with every word in `Key`.

The second step is an operation between this intermediate ‘factor’ matrix and the Value (V) matrix, to produce the attention score that is output by the attention module.

For each *individual* row $i$, in the `Attention Score Matrix` ($S$), we have an expression $\textnormal{softmax}(\frac{1}{\sqrt{d_k}} \lang \vec{q}_i\cdot\vec{k}_0, \vec{q}_i \cdot \vec{k}_1, \dots, \vec{q}_i \vec{k}_n \rang)$ = $\frac{1}{N} ( e^{\frac{\vec{q}_i \cdot \vec{k}_0}{\sqrt{d_k}}}, e^{\frac{\vec{q}_i \cdot \vec{k}_1}{\sqrt{d_k}}}, \dots , e^{\frac{\vec{q}*i \cdot \vec{k}*n}{\sqrt{d_k}}})$. $N$ is the `"normalization constant"`,

$$N = \displaystyle\sum_{j=0}^n e^{\frac{\vec{q}_i\vec{k}_j}{\sqrt{d}_k}}$$

### The Feed-forward Neural Network



## Appendix

### How Softmax is calculated

> The softmax function is used to convert a vector of real numbers into a probability distribution. Given an input vector $x$ of length $n$, the softmax function $σ$ is defined as $σ(x)_i = exp(x_i) / Σ_j exp(x_j)$

Here, $exp$ is the `exponential` function, $x_i$ is the $i^{th}$ element of the input vector $x$, and the ***denominator*** $Σ_j exp(x_j)$ is the sum of the exponentials of *all* elements in the vector.
