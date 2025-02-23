---
layout: post
title: Contrastive Chain-of-Thought Prompting
# thumbnail: /assets/img/weekly-review/nlp_cover_week_46.jpg
categories: [papers, weekly-review, nlp]
description: ""
toc:
  beginning: true
featured: false
---


## [Contrastive Chain-of-Thought Prompting][contrastiveCotPaper]



 
### Introduction
In the landscape of large language models (LLMs), the success of Chain of Thought (CoT) in bolstering reasoning capabilities is apparent, yet the intricacies of its underlying processes remain unclear. Despite logical reasoning being seemingly crucial, the conventional CoT prompts have shown minimal differentiation between valid and invalid demonstrations. This paper grapples with the challenge of understanding and enhancing CoT by proposing the innovative concept of contrastive chain of thought. Drawing inspiration from human learning, the authors introduce both positive and negative examples in the form of contrastive demonstrations to guide LLMs through step-by-step reasoning, addressing the limitations of the traditional CoT approach.

### Research Gap

The research gap addressed in this paper revolves around the limitations of the existing CoT prompting method for enhancing the reasoning abilities of large language models. Despite the success of CoT, the authors highlight a lack of comprehensive understanding of its underlying processes, particularly concerning the minimal impact observed when using invalid demonstrations and the absence of guidance on mistake avoidance during reasoning. To bridge this gap, the paper proposes Contrastive CoT, aiming to leverage both positive and negative examples by introducing contrastive demonstrations. This approach seeks to improve the language model's reasoning step-by-step, addressing the identified limitations and providing a more effective enhancement to the CoT method.


### Solution: Contrastive CoT

To address the problem at hand, the authors begin by deconstructing CoTs into two fundamental components: __bridging objects__ and __language templates__. Bridging objects serve as symbolic elements, encompassing numbers, equations in arithmetic tasks, and names or entities in factual tasks. Language templates act as contextual textual hints for bridging objects.

In the process of transforming a CoT into a contrastive (invalid) version, several key aspects come into focus, namely __coherence__ and __relevance__. Coherence involves ensuring the correct sequencing of reasoning steps, while relevance ensures that the CoT contains pertinent information. For instance, if a question revolves around a person named Leah, the corresponding CoT should also specifically reference Leah, not any other person unrelated to the question. Both coherence and relevance are considered for both bridging objects and language templates.

To create a contrastive CoT, the authors introduce intentional disruptions either by altering the order of reasoning steps, thereby compromising coherence, or by replacing entities in the reasoning steps with irrelevant and random entities, thus compromising relevance. The paper illustrates various methods of creating contrastive CoTs from a genuine CoT in the figure below:

<p style="text-align:center;"><img src="/assets/img/weekly-review/contrastive_cot_example.png" alt="The Architecture" width="850" height="500"></p>

__My Comment:__ It's worth mentioning that the concept of using contrastive CoTs predates this paper by about a year. However, the innovation lies in the paper's introduction of an automated process for generating such contrastive CoTs, although the perceived research-wise innovation of this automation is questioned.

### Experiments

The primary aspects of the experimental setups are as follows:

* **Tasks:** The experiments in the paper cover two main types of reasoning tasks: arithmetic reasoning and factual question answering (QA).
* **Dataset:** For arithmetic reasoning, the authors evaluate on datasets including GSM8K, AQuA, GSM-Hard, SVAMP, and ASDIV. Factual QA tasks include Bamboogle and StrategyQA.
* **Baselines:** GPT-3.5-Turbo with simple CoT or without CoT at all.
* **Evaluation Metrics:**  The performance is measured using different evaluation metrics specific to each task. Notable improvements are observed in tasks such as GSM-8K and Bamboogle, with gains of 9.8 and 16.0 points, respectively, demonstrating the effectiveness of the proposed contrastive chain of thought method.


### Conclusion 
In conclusion, this paper pioneers the introduction of contrastive CoT as a robust enhancement to traditional CoT, marking a significant step toward unraveling the intricacies of language model reasoning. By systematically incorporating both valid and invalid demonstrations, the proposed approach proves its mettle across various reasoning tasks and benchmarks. The automated creation of contrastive CoTs, although building upon a pre-existing concept, streamlines the process, providing an efficient and effective means of guiding language models through complex reasoning steps. The demonstrated improvements in tasks such as GSM-8K and Bamboogle underscore the potential of contrastive chain of thought as a versatile and impactful enhancement to existing CoT methods.




[contrastiveCotPaper]: https://arxiv.org/pdf/2311.09277.pdf
[contrastiveCotSum]: /blog/2023/week-46/#contrastive-chain-of-thought-prompting
