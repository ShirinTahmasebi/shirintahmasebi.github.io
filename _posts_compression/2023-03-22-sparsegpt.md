---
layout: post
title: SparseGPT
thumbnail: /assets/img/cover/sparsegpt.jpg
categories: []
tags:
  - compression
  - pruning
description: >
    Making Smaller LLMs in One-shot
toc:
  beginning: true
---


## [SparseGPT: Massive LLMs Can be Accurately Pruned in One-Shot][sparsegptPaper]


### Introduction

SparseGPT is a **one-shot pruning method** that enables **massive language models** (e.g., GPT-3-scale) to be pruned **without retraining or fine-tuning**. Unlike previous pruning techniques, which require multiple training steps to recover accuracy, SparseGPT efficiently removes **50-60% of weights** while maintaining **low accuracy loss**.  

This pruning method can be categorized based on:  
1. **Granularity**: SparseGPT performs **unstructured pruning** (removes individual weights), but it also supports **semi-structured pruning** (e.g., **2:4, 4:8 sparsity**, which is optimized for GPU acceleration).  
2. **Pruning Criteria**: Instead of simply removing small-magnitude weights, SparseGPT **estimates weight importance using the Hessian inverse** and removes the **least important** ones.  
3. **Pruning Ratio**: It achieves **50-60% sparsity** with minimal impact on model accuracy.  
4. **Need for Retraining**: **No retraining or fine-tuning is required**. It works in a **one-shot** manner, pruning **each layer individually** while adjusting the remaining weights.


### Research Gap

Prior to SparseGPT, **pruning large-scale language models** (100B+ parameters) was **an unsolved problem** due to two major challenges: **scalability and accuracy recovery**. Most pruning methods required **iterative fine-tuning** to regain accuracy after removing weights, making them computationally infeasible for **GPT-scale models**. Even existing **one-shot pruning methods** were limited in effectiveness because they either relied on **simple magnitude pruning** (which severely degrades accuracy at high sparsity levels) or were **too expensive to apply** to models with billions of parameters.  

SparseGPT **fills this gap** by introducing a **scalable one-shot pruning method** that can **prune extremely large models** without fine-tuning while preserving performance. The method is designed to handle **models with 100+ billion parameters** efficiently, achieving **high sparsity (50-60%)** without requiring costly retraining. This work is the **first to demonstrate** that GPT-scale models can be **accurately pruned in one shot**, making pruning **practical for real-world deployment**.

### SparseGPT

1. Layer-wise Pruning:  
   - It **prunes one layer at a time** rather than the whole network simultaneously.  

2. Selecting Which Weights to Prune (Using Hessian Inverse):
   - The **Hessian matrix** captures how each weight **affects the loss function**.  
   - SparseGPT **approximates the inverse of this Hessian**, which helps measure the **sensitivity of the loss function** to each weight by calculating **Hessian matrix**.  
   - It **removes the least important weights**, meaning those that have the lowest impact on the layer’s output.  

3. Reconstructing the Remaining Weights (Using Sparse Regression): 
   - After pruning, the layer's output **would normally change**, which could hurt accuracy.  
   - To fix this, SparseGPT **solves a sparse regression problem**:  
     - **Sparse regression** is a technique that **finds the best values for the remaining weights** while minimizing the squared error between the **original layer output and the pruned layer output**.  
   - This adjustment ensures that the **pruned layer behaves similarly to the original**.  

4. One-Shot Execution:  
   - This process is done **once per layer**, moving through the network **without retraining**.  
   - The entire model can be pruned in just a few hours on a single GPU, even for 100+ billion parameter models.


### Conclusion


SparseGPT is important because it enables highly efficient pruning of massive GPT models while avoiding expensive retraining. It is particularly valuable for large-scale models like OPT-175B and BLOOM-176B, where standard pruning approaches either fail or require enormous computational resources. By allowing 50-60% sparsity while maintaining low accuracy loss, SparseGPT makes large models more efficient and easier to deploy.  

SparseGPT achieves this by measuring weight importance using a Hessian inverse approximation to determine sensitivity to loss, removing the least important weights, and recalculating the remaining ones using sparse regression to minimize accuracy loss. Since pruning happens layer by layer in one-shot, it does not require fine-tuning or multiple training iterations.  

Additionally, SparseGPT supports hardware-friendly sparsity patterns (such as 2:4 and 4:8 sparsity) that make it compatible with modern GPU acceleration techniques. Furthermore, it can be combined with quantization to further reduce memory and computation costs.  

By making large language models faster, smaller, and more efficient, SparseGPT is a practical and scalable solution for LLM compression, enabling cost-effective deployment without sacrificing accuracy.

[sparsegptPaper]: https://arxiv.org/pdf/2301.00774


