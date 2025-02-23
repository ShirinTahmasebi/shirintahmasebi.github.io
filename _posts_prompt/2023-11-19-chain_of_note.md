---
layout: post
title: Chain-of-Note
# thumbnail: /assets/img/weekly-review/nlp_cover_week_46.jpg
categories: []
tags:
  - llm
  - prompt
description: >
    Enhancing Robustness in Retrieval-Augmented Language Models
toc:
  beginning: true
featured: false
---


## [Chain-of-Note: Enhancing Robustness in Retrieval-Augmented Language Models][conPaper]

 

 
### Introduction


Retrieval-Augmented Language Models (RALMs) represent a significant advancement in NLP, leveraging external knowledge sources to enhance LLMs' capabilities. However, existing RALMs face challenges related to the reliability of retrieved information and the models' ability to assess their own knowledge adequacy. In particular, the retrieval of irrelevant data can lead to misguided responses, and standard RALMs often struggle to determine whether they have sufficient knowledge, both intrinsic and retrieved, to provide accurate answers. These limitations highlights the research gap addressed in this paper. To tackle these challenges, the authors propose CHAIN-OF-NOTE (CON), a novel framework designed to improve the robustness of RALMs, specifically focusing on noise and unknown robustness. CON introduces a systematic approach to evaluating the relevance of retrieved documents through the generation of sequential reading notes, enabling an accurate assessment of their significance to a given question.

### Research Gap


The research gap addressed by this paper is about the limitations of existing RALMs, particularly in terms of noise and unknown robustness. Standard RALMs face challenges in ensuring the reliability of retrieved information, with the risk of providing misguided responses due to irrelevant data. Additionally, these models often struggle to assess their own knowledge adequacy and lack a mechanism to handle unknown scenarios, where the answer cannot be determined. The proposed CON framework aims to fill this gap by introducing a novel approach to improve the robustness of RALMs. CON focuses on two pivotal aspects: __noise robustness__, by discerning and disregarding noisy information in irrelevant documents, and __unknown robustness__, by enabling RALMs to respond with _unknown_ when faced with queries outside their knowledge scope.

### Solution: CHAIN-OF-NOTE (CON)

The proposed solution, CON,  addresses the robustness of Retrieval-Augmented Language Models (RALMs) by integrating relevant external knowledge while addressing challenges associated with _noisy_ or _irrelevant_ data. 
The system takes questions and associated Wikipedia documents, extracted from the NQ dataset, as input. These documents are processed by ChatGPT to generate summaries, which are then used as sequential reading notes. The resulting dataset comprises questions and their corresponding notes, designed to train a robust LLaMa model. 

To simulate noisy and irrelevant scenarios, certain questions are paired with mismatched or irrelevant notes, requiring the model to rely on its intrinsic knowledge for accurate answers.

The training loss is determined by the next token prediction accuracy of the total answer, encompassing explanations and the final answer. 

This approach enhances the model's ability to handle noisy data, irrelevant information, and situations where the answer is unknown, thereby improving the overall robustness of RALMs.



Three sample scenarios are mentioned in the following figure:

<p style="text-align:center;"><img src="/assets/img/weekly-review/con_sample.png" alt="The Architecture" width="850" height="500"></p>




### Experiments

The primary aspects of the experimental setups are as follows:

* **Tasks:** Question-Answering
* **Dataset:** The experiments were conducted across four open-domain question-answering benchmarks: Natural Questions (NQ)—mainly used for creating the dataset, TriviaQA, WebQ, and RealTimeQA.
* **Baselines:** Standard RALMs without CON
* **Evaluation Metrics:** Evaluation metrics included Exact Match (EM) score, measuring the percentage of model-generated answers that exactly match the ground truth. For assessing noise robustness, the improvement in accuracy (EM score) with noisy retrieved documents was considered. Unknown robustness was assessed through an increase in rejection rates. This involved evaluating the model's ability to reject providing an answer when faced with real-time questions that were beyond the pre-training knowledge scope.

### Conclusion

In conclusion, the CHAIN-OF-NOTE (CON) framework presents a promising solution to the limitations of standard RALMs. Through systematic evaluation using sequential reading notes, CON significantly enhances RALMs' robustness in handling noisy and irrelevant documents, as well as addressing unknown scenarios. Experiments conducted across multiple open-domain QA benchmarks demonstrate notable improvements, with CON-equipped RALMs outperforming their standard counterparts. 


[conPaper]: https://arxiv.org/pdf/2311.09210.pdf
[conSum]: /blog/2023/week-46/#chain-of-note-enhancing-robustness-in-retrieval-augmented-language-models