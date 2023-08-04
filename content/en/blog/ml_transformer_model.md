---
author: "Benji Peng"
title: "Transformer Model"
description: "An attempt to explore the intricate interplay among diverse industries, from infrastructure to consumer services"
tags: ["branding", "profile", "fundamentals"]
date: 2023-08-02
thumbnail: https://raw.githubusercontent.com/benjipeng/assets/main/rc/blog/ml/llm/transformer.jpg
math: true
---

> Transformers are a type of model architecture used in machine learning, particularly in the field of natural language processing (NLP). They were introduced in a paper titled **'Attention is All You Need'** by `Vaswani et al.` in **2017**. The main innovation of the Transformer model is the self-attention mechanism, which allows the model to weigh the importance of words in an input sequence when making predictions.

A *typical* Transformer model contains:

1. **Input Embedding Layer**: This is where the input data (usually text data) is converted into a numerical form that the model can understand. Each word (or token) in the input data is represented as a high-dimensional vector in this embedding space.

2. **Positional Encoding**: This is a step unique to Transformers. Because Transformers do not inherently understand the order of the data (unlike RNNs or LSTMs), positional encoding is added to give the model information about the position of the words in the input sequence.

3. **Encoder**: The encoder takes the embedded input data and processes it using self-attention and feed-forward neural networks. The Transformer model typically has multiple layers of these encoders.

    - Self-Attention: This mechanism allows the model to weigh the importance of words in the input sequence. For each word, it determines a score indicating how much attention should be paid to each other word when predicting the next word.

    - Feed-Forward Neural Networks: These are standard fully connected neural networks that follow the self-attention mechanism in each encoder layer.

4. **Decoder**: The decoder also consists of layers of self-attention and feed-forward neural networks, but with an additional layer of cross-attention that pays attention to the output of the encoder. The decoder generates the output sequence.

5. **Output Linear Layer and Softmax**: The output from the decoder is passed through a linear layer followed by a softmax function to generate the final output probabilities for each possible next word in the sequence.

## Mathematical Representation

### Self-attention Mechanism

> The `self-attention mechanism` in the Transformer model calculates the **attention score** for *each* word in the `input sequence` with respect to `every other word`. This is done using the `Query`($Q$), `Key` ($K$), and `Value` ($V$) vectors that are learned during training.

1. Calculate `Query`, `Key`, and `Value` **vectors**: For each word in the input sequence, we calculate the Query, Key, and Value vectors. These are obtained by multiplying the embedding of the word by the learned weight matrices ($W_Q$, $W_K$, $W_V$).

2. Calculate attention scores: The attention score matrix ($S$) is calculated as the **cross product** of the **Query matrix** and the ***transpose*** of the **Key matrix**. This results in a matrix of shape `(n, n)`, where each element $S_{ij}$ represents the raw attention score between word i and word j. This is then divided by the square root of the dimension of the key vectors (d_k) to give more importance to the lower values and to stabilize the gradients.

TO BE CONTINUED

$\int_{a}^{b} x^2 dx$

## Appendix

### How Softmax is calculated

> The softmax function is used to convert a vector of real numbers into a probability distribution. Given an input vector $x$ of length $n$, the softmax function $σ$ is defined as $σ(x)_i = exp(x_i) / Σ_j exp(x_j)$

Here, $exp$ is the `exponential` function, $x_i$ is the $i^{th}$ element of the input vector $x$, and the ***denominator*** $Σ_j exp(x_j)$ is the sum of the exponentials of *all* elements in the vector.
