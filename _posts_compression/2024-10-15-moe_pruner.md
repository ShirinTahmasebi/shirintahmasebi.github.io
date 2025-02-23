---
layout: post
title: MoE-Pruner
thumbnail: /assets/img/cover/moe_pruner.jpg
categories: []
tags:
  - compression
  - pruning
description: >
    Pruning MoE LLMs
toc:
  beginning: true
---


## [MoE-Pruner: Pruning MoE LLMs using the Hints from Its Router][moe_prunerPaper]


### Introduction

As the size of LLMs continues to expand, their computational and memory requirements have become increasingly prohibitive for both training and inference. Mixture-of-Experts (MoE) architectures offer a promising solution by introducing sparse activation, where only a subset of model parameters is utilized at any given time. However, while MoE significantly reduces inference costs compared to fully dense models, it also introduces new challenges such as expert redundancy, inefficient parameter utilization, and increased memory demands. Pruning techniques have been widely used to compress dense transformers, but standard pruning approaches fail to leverage the unique structure of MoE models, where a router dynamically assigns tokens to different experts. To address this, MoE-Pruner introduces a router-aware pruning strategy that incorporates routing weights into the pruning metric, ensuring that important experts remain intact while redundant ones are eliminated. Additionally, an expert-wise knowledge distillation method is employed to recover lost performance post-pruning, making this technique both effective and computationally efficient.


This pruning method can be categorized based on:  
1. **Granularity**:  Unstructured pruning at the weight level inside experts. It is worth mentioning that while it is not strictly a structured pruning method (e.g., removing entire neurons, layers, or experts), its router-aware mechanism makes it aware of expert importance, making it different from fully unstructured pruning methods like SparseGPT or Wanda.
2. **Pruning Criteria**: Uses a router-informed metric that combines weight magnitude, input activation norms, and router probabilities to prioritize pruning decisions.
3. **Pruning Ratio**: Supports high sparsity levels (e.g., 50%) while preserving most of the model's original performance.
4. **Need for Retraining**: One-shot pruning that does not require retraining, but performance can be enhanced via expert-wise knowledge distillation.



### Backgroud and Research Gap

As LLMs continue to grow in size, their computational and memory requirements have become a major bottleneck for both training and inference. To address these challenges, Mixture-of-Experts (MoE) architectures have emerged as an efficient alternative to fully dense transformer models. Instead of activating all parameters for every input, MoE selectively routes different tokens to different experts, significantly reducing computation while maintaining model expressiveness.

At its core, an MoE model consists of three key components: a router (gating network), a set of experts (feedforward networks), and a sparse activation mechanism. Each token in an input sequence is first processed by the router, which assigns a probability distribution over all available experts. Instead of utilizing all experts for every token, the router typically selects the top-K experts (often Top-2) with the highest probability scores. These selected experts then process the token, and their outputs are weighted and aggregated based on the router's assignment.

This token-level routing mechanism allows MoE models to scale efficiently while keeping computational costs manageable. Unlike traditional transformers, where every token flows through the same fully connected layers, MoE models enable specialization by allowing different experts to learn distinct representations. For instance, some experts may specialize in syntactic structures, while others focus on semantic relationships or domain-specific knowledge. This targeted processing contributes to improved model efficiency and better generalization across diverse tasks.

#### Mixtral: A High-Performance MoE Model
One of the most notable recent MoE architectures is Mixtral, an open-source model developed as a successor to LLaMA. Mixtral-8x7B, for example, consists of eight experts, but only two experts are active per token. This means that while the full model has 47 billion parameters, only 12.5 billion are used at a time, making inference significantly more efficient than a dense model of the same size.

In Mixtral, the router is responsible for selecting which two experts will process each token. The router assigns probability scores to each expert using a learned gating function, ensuring that different tokens are routed to the most relevant experts. Each expert in Mixtral is essentially a feedforward network (FFN), similar to those found in dense transformers but trained to specialize in different types of linguistic patterns. This enables Mixtral to achieve strong generalization and efficiency, making it one of the leading MoE models available.

#### MoE Disadvantage
Despite its advantages, MoE presents unique challenges. The presence of redundant or underutilized experts can lead to wasted computational resources, and imbalanced expert utilization—known as representation collapse—can reduce model effectiveness. Furthermore, MoE architectures require increased memory bandwidth due to the need for maintaining multiple experts. To mitigate these issues, various strategies such as expert pruning, expert merging, and knowledge distillation have been explored to optimize MoE models while preserving their performance.

#### Research Gap

While MoE models like Mixtral offer computational efficiency by selectively activating only a subset of experts, they introduce new challenges related to expert redundancy and underutilization. Many MoE models suffer from representation collapse, where certain experts dominate while others remain underused, leading to inefficiencies in parameter utilization. Traditional pruning methods like SparseGPT and Wanda fail to address this issue because they treat all weights equally, without considering how frequently experts are used.

To bridge this gap, MoE-Pruner introduces a router-aware pruning strategy that leverages router weights to inform pruning decisions. By incorporating routing probabilities into the pruning metric, MoE-Pruner ensures that frequently used experts are preserved while less critical ones are pruned, making it more effective than traditional weight-based pruning techniques. Furthermore, expert-wise knowledge distillation helps recover model performance post-pruning, making this method particularly well-suited for optimizing MoE-based LLMs.


### Solution: MoE-Pruner

MoE-Pruner is an **extension of Wanda**, specifically designed for **pruning MoE models** by incorporating **router weights** into the pruning process. While Wanda prunes weights based on **magnitude and input activations**, MoE-Pruner enhances this approach by **leveraging the routing probabilities**, ensuring that frequently used experts are preserved while less utilized ones are pruned more aggressively.  

The pruning process starts by analyzing the **MoE routing mechanism**, where each input token is assigned to a subset of experts based on learned gating probabilities. These router weights indicate how often each expert is utilized, providing a **critical signal** for pruning decisions. Instead of treating all weights equally, MoE-Pruner extends Wanda's pruning metric by introducing a **router-aware importance score**:

$$
S = \vert W_{ij} \vert \times \|X_j \times G_j\|
$$

where:
- $$W_{ij}$$ is the weight magnitude,  
- $$X_j$$ is the input activation norm,  
- $$G_j$$ is the router weight assigned to the expert.  

This adjustment ensures that weights within highly utilized experts are pruned more conservatively, while those in **rarely activated experts** are **pruned more aggressively**.  

Pruning is applied **at the weight level within each expert**, making it an **unstructured pruning method**. Unlike structured expert pruning approaches that remove entire experts, MoE-Pruner preserves all experts but naturally reduces capacity in underutilized ones by pruning more of their parameters.  

Since pruning can introduce some performance degradation, MoE-Pruner includes an **expert-wise knowledge distillation** step. After pruning, the compressed model is fine-tuned using the original pretrained MoE model as a **teacher**, transferring knowledge back to the pruned model. This enables the pruned model to retain nearly all of its original performance, despite having significantly fewer active weights.  

In summary, MoE-Pruner is a **router-aware extension of Wanda**, designed to optimize MoE models by pruning weights intelligently based on both input activations and expert utilization. By integrating routing information into the pruning process, it achieves better compression with minimal performance loss, outperforming traditional LLM pruning techniques that do not account for expert specialization.

### Conclusion


MoE-Pruner extends Wanda by introducing a router-aware pruning strategy, making it better suited for MoE models. By incorporating routing probabilities into the pruning metric, it ensures that frequently used experts retain more parameters, while underutilized experts are pruned more aggressively. This unstructured, one-shot pruning method reduces computational overhead while maintaining strong model performance. Additionally, expert-wise knowledge distillation helps recover any accuracy loss post-pruning. As MoE models become more prevalent, MoE-Pruner provides an effective approach for optimizing their efficiency without sacrificing their expressiveness.











[moe_prunerPaper]: https://arxiv.org/pdf/2410.12013