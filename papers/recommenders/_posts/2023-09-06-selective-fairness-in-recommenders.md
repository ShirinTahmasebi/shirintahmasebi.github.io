---
layout: post
title: Selective Fairness in Recommendation via Prompts
image:
    path: /assets/img/weekly-review/selective_fairness_cover.jpg
categories: [papers, nlp]
description: >
    Selective Fairness Paper Review
sitemap: false
hide_last_modified: true
---

In this post, I am going to review the [PFRec][PFRec] paper. The paper focuses on the challenge of recommendation fairness, particularly in scenarios where users have multiple sensitive attributes, such as age, gender, or country, and they may want to customize the fairness of their recommendations based on these attributes. 

The key contribution of this work is the introduction of a selective fairness task, where users can flexibly choose which sensitive attributes should be considered in making recommendations. To address this, the paper proposes a novel parameter-efficient framework called Prompt-based Fairness-Aware Recommendation (PFRec), which leverages Transformer-based models and adapters for fairness-aware tuning. The contributions of the paper include highlighting the challenges of selective fairness, introducing personalized attribute-specific prompts, and demonstrating the effectiveness of PFRec in various fairness scenarios.

It's important to note that the paper distinguishes between two main phases: pre-tuning and fine-tuning. Pre-tuning focuses on training a Transformer-based model to predict the next user interaction by processing user interactions one at a time. During this phase, the Binary Cross-Entropy (BCE) loss is used to update the weights of the Transformer.

In the pre-tuning phase, the Transformer processes user interactions sequentially, aiming to predict the next interaction in the sequence. The BCE loss is employed to update the weights of the Transformer. The Transformer architecture typically consists of multiple layers and attention mechanisms, and it captures sequential patterns in user behavior.

Following the pre-tuning phase, the Transformer's weights are frozen, and the fine-tuning phase, which is bias-aware, begins. The goal of fine-tuning is to address fairness concerns, specifically for the $$k$$-th combination of user-sensitive attributes. To achieve this, a prompt is generated based on the k-th attributes and passed to an adapter-augmented Transformer.

The adapter is introduced as a linear transformation applied to the LayerNorm of the Transformer's input. This adapter architecture significantly reduces the number of parameters that need updating compared to fine-tuning the entire Transformer. The adapter's parameters are optimized to make the user representation produced by the adapter-augmented Transformer as bias-free as possible with respect to the selected attributes.

To further enhance fairness, an adversarial training component is introduced. An adversary classifier takes the user representation generated by the adapter-augmented Transformer and attempts to predict the user's sensitive attributes based on this representation. The loss of the adversary classifier, along with the BCE loss of the adapter-augmented Transformer, forms the overall loss used to update the adapter. Adversarial training encourages the adapter to reduce biases in the user representation, as a successful adversary indicates remaining biases.

In summary, the paper presents a novel framework, PFRec, which allows for selective fairness customization in recommendation systems. It achieves this through a two-phase approach, where pre-tuning focuses on sequence prediction and fine-tuning introduces adapters and adversarial training to address bias-aware fairness concerns.


Now, in what follows, the used datasets and the experiments are explained. The experiments in the paper are conducted using two public datasets: **CIKM** and **AliEC**. The CIKM dataset is an E-commerce recommendation dataset. It has 88,000 items and 60,000 users with 2.1 million click instances. Each user has 3 attributes: gender, age, consumption level. The AliEC dataset contains nearly 100,000 users and 8.8 million click instances. Each user has two attributes gender and age. 


Two benchmars are designed for evaluation:
* **Single-Attribute Fairness:** The paper evaluates the proposed PFRec framework on both single-attribute fairness settings and multi-attribute fairness settings. Single-attribute fairness focuses on considering the fairness of recommendations with respect to a single user-sensitive attribute, such as gender or age.

* **Multi-Attribute Fairness:** In addition to single-attribute fairness, the paper also examines multi-attribute fairness settings. This means that the fairness-aware modeling can take into account multiple user-sensitive attributes simultaneously to provide fairness in recommendations.

The paper compares the performance of PFRec with several baselines, including:
* **SASRec:** This is the same pre-training model used for learning user representations, without any fairness considerations. It serves as a baseline for evaluating PFRec's fairness-aware capabilities.
* **BERT4Rec:** BERT4Rec is mentioned as another strong BERT-based sequential recommendation model. While the paper doesn't provide specific details, it implies that BERT4Rec is one of the models used for comparison with PFRec.

Performance measurement metrics can be divided into two categories:

1. Accuracy Metrics:
    * **HIT@N**: HIT@N measures the accuracy of the top-N recommendations. It calculates how many of the user's actual interactions are included in the top-N recommended items. 

    * **NDCG@N** (Normalized Discounted Cumulative Gain): NDCG@N is another ranking-based metric that evaluates the quality of the top-N recommendations. It takes into account both the relevance and ranking of recommended items. 

1. Fairness Metrics:
    * **F1**: To evaluate fairness performance, the paper employs micro-averaged F1 of the adversarial classifier. This metric is used to measure how well the model achieves fairness in recommendations. Since the adversarial classifier is used to measure F, smaller values of F1 indicate better fairness performance. 
    

[PFRec]: https://arxiv.org/pdf/2205.04682.pdf