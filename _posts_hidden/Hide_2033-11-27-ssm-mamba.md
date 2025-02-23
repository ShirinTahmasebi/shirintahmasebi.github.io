---
layout: post
title: State Space Models
thumbnail: /assets/img/weekly-review/nlp_cover_ssm.jpg
categories: [papers, nlp]
description: >
    An overview of State Space Models and comparing them with other sequential models
toc:
  beginning: true
featured: true
---

Here is the list of the most interesting papers published in this week:
* [Mamba: Linear-Time Sequence Modeling with Selective State Spaces][mambaSum]


But, the main focus of this blog post is to introduce and explain the Mamba model in details.


---

## [Mamba: Linear-Time Sequence Modeling with Selective State Spaces][mambaPaper]
 

### Introduction

In the landscape of machine learning and signal processing, various models have been developed to tackle the challenges posed by Long-Range Dependency (LRD) in sequential data. Among these, Recurrent Neural Networks (RNNs), Long Short-Term Memory networks (LSTMs), Transformers, and State Space Models (SSMs) stand out for their ability to process sequences of data over time. A common thread that binds these models is their recursive nature, allowing them to maintain a form of memory or state across time steps, which is crucial for understanding and predicting sequences with long-range dependencies.

Each of these models are different in their structure and are also coming from various origins and domains. For example:

* **RNNs and LSTMs** originate from the domain of neural networks in machine learning. RNNs are fundamental to this area, designed specifically for sequential data. LSTMs, an advanced form of RNNs, were developed to address the vanishing gradient problem in standard RNNs, thereby enhancing the model's ability to capture longer dependencies.
* **Transformers** emerged from the field of natural language processing (NLP). They represent a paradigm shift, focusing on self-attention mechanisms to weigh the significance of different parts of the input data. This approach allows them to handle sequences with far greater efficiency, particularly in tasks like language translation and text generation.
* **SSMs** have their roots in control theory and time series analysis. These models are structured to represent dynamic systems, capturing the evolution of observable outputs as a function of underlying state variables.

All these models, at their core, deal with data sequentially, updating their internal states or representations as new data arrives. This recursive processing is key to their effectiveness in LRD scenarios.


### Comparing Sequential Models

Before introducing SSM, let us have a quick review over the popular and famous architectures for sequential processing—which are CNNs, RNNs, LSTMs, and Transformers.

|                                           | CNN              | RNN        | LSTM       | Transformer   | <span style="color: #50A492">Ideal</span>         |
|-------------------------------------------|------------------|------------|------------|---------------|---------------|
| Parallel or Sequential in Training        | Parallel         | Sequential | Sequential | Parallel      | <span style="color: #50A492">Parallel</span>      |
| Parallel or Sequential in Inference       | Parallel         | Sequential | Sequential | Sequential    | <span style="color: #50A492">Parallel</span>      |
| Computational Complexity                  | $$O(n \cdot k)$$ | $$O(n)$$   | $$O(n)$$   | $$O(n^2)$$    | <span style="color: #50A492">$$O(n)$$</span>       |
| Having Autoregressive Generation          | No               | Yes        | Yes        | Yes           | <span style="color: #50A492">Yes</span>           |
| Ability of Handling Long-Range Dependency | Limited          | Limited    | Good       | Excellent     | <span style="color: #50A492">Excellent</span>      |
| Processing Time for Each Token            | Constant         | Constant   | Constant   | Not Constant  | <span style="color: #50A492">Constant</span>      |


<p></p>

- **Parallel or Sequential in Training**: 
	- <span style="color: #b71c1c; font-weight: bold">CNN:</span> Parallel - They can process multiple components of the data simultaneously.
	- <span style="color: #b71c1c; font-weight: bold">RNN:</span> Sequential - They process data points in the sequence one after another.
	- <span style="color: #b71c1c; font-weight: bold">LSTM:</span> Sequential - They are similar to RNNs, but with more complex internal structures.
	- <span style="color: #b71c1c; font-weight: bold">Transformers:</span>  Parallel - They process entire sequences simultaneously.


- **Parallel or Sequential in Inference**: 
	- <span style="color: #b71c1c; font-weight: bold">CNN:</span> Parallel - Convolution operations are inherently parallelizable.
	- <span style="color: #b71c1c; font-weight: bold">RNN:</span>  Sequential - Each output depends on the previous computation.
	- <span style="color: #b71c1c; font-weight: bold">LSTM:</span>  Sequential - Similar to RNNs, each step depends on the previous one.
	- <span style="color: #b71c1c; font-weight: bold">Transformers:</span>  Sequential for autoregressive models—such as GPT, parallel for others--such as BERT. It depends on the task and model type.


- **Computational Complexity w. r. t. Input Length $$n$$**: 
	- <span style="color: #b71c1c; font-weight: bold">CNN:</span> Generally $$O(n \cdot k)$$ for 1D convolutions, where $$n$$ is the input length and $$k$$ is the kernel size. CNNs process each convolutional window (of size $$k$$) independently, and the complexity scales linearly with the length of the input sequence. The complexity is generally lower for small kernel sizes but can increase with deeper or more complex architectures.
	- <span style="color: #b71c1c; font-weight: bold">RNN:</span> $$O(n)$$ - RNNs process each token sequentially, with the complexity scaling linearly with the sequence length. Each token's processing involves matrix operations that are independent of the sequence length.
	- <span style="color: #b71c1c; font-weight: bold">LSTM:</span> $$O(n)$$, similar to RNNs - While LSTMs have a more complex structure than simple RNNs (due to additional gates), the computational complexity still scales linearly with the input length, as each token is processed sequentially.
	- <span style="color: #b71c1c; font-weight: bold">Transformers:</span>  $$O(n^2)$$ The self-attention mechanism in Transformers requires comparing each token with every other token, leading to quadratic complexity with respect to the sequence length.


- **Having Autoregressive Generation**: 
	- <span style="color: #b71c1c; font-weight: bold">CNN:</span> No - CNNs are generally not used for sequential data generation.
	- <span style="color: #b71c1c; font-weight: bold">RNN:</span> Yes - They are naturally suited for generating sequences.
	- <span style="color: #b71c1c; font-weight: bold">LSTM:</span> Yes - They are effective for sequence generation tasks.
	- <span style="color: #b71c1c; font-weight: bold">Transformers:</span>   Yes - Particularly in models like GPT for text generation.

- **Ability of Handling Long-Range Dependency**: 
	- <span style="color: #b71c1c; font-weight: bold">CNN:</span> Limited - They mostly focus on local patterns.
	- <span style="color: #b71c1c; font-weight: bold">RNN:</span> Limited - Basic RNNs struggle with long-term dependencies due to vanishing gradient problem.
	- <span style="color: #b71c1c; font-weight: bold">LSTM:</span> Good - Gating mechanism helps in maintaining long-term dependencies.
	- <span style="color: #b71c1c; font-weight: bold">Transformers:</span>  Excellent - Self-attention mechanism captures dependencies regardless of distance.

- **Processing Time for Each Token**: 
	- <span style="color: #b71c1c; font-weight: bold">CNN:</span> Constant - The processing time per token is constant due to the fixed convolutional operations.
	- <span style="color: #b71c1c; font-weight: bold">RNN:</span> Constant - All tokens are processed in a similar manner to each other.
	- <span style="color: #b71c1c; font-weight: bold">LSTM:</span> Constant - All tokens are processed in a similar manner to each other.
	- <span style="color: #b71c1c; font-weight: bold">Transformers:</span> Not Exactly Constant - The processing time for each token during generation in a Transformer is not strictly constant, as the self-attention mechanism considers the entire preceding context. As the sequence gets longer, the context that the self-attention mechanism needs to consider grows.


[mambaPaper]: https://arxiv.org/abs/2312.00752
[mambaSum]: /blog/2023/ssm-mamba/#mamba-linear-time-sequence-modeling-with-selective-state-spaces
