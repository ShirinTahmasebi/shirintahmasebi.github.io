---
layout: post
title: Recommendation as Instruction Following (InstructRec)
# thumbnail: /assets/img/weekly-review/nlp_cover_week_19.jpg
categories: []
tags:
  - recsys
  - llm
description: >
    A Large Language Model Empowered Recommendation Approach
toc:
  beginning: true
---


## [Recommendation as Instruction Following: A Large Language Model Empowered Recommendation Approach][InstructRecPaper]

In this paper, the authors are focusing on using _instruction following_, one of the most interesting capabilities of Large Language Models (LLM), to improve the existing recommendation systems. The main reason behind that is that by using the instruction-following capability, it would be possible to create a recmmendation system with generalization ability to be able to handle the unseen data. 

To create such instructions, they take three aspects into account:
* **User Preference (P)**, which is the inherent and long-term user preferences. User preferences can be divided into three different levels:
  * _None_ (P<sub>0</sub>)
  * _Implicit Preference_ (P<sub>1</sub>)
  * _Explicit Preference_ (P<sub>2</sub>)


* **Intention (I)**, which is the immediate demands of the user for certain types of items. The user intention can be different from user long-term preferences. User intentions can be divided into three different levels:
  * _None_ (I<sub>0</sub>)
  * _Vague Intention_ (I<sub>1</sub>)
  * _Specific Intention_ (I<sub>2</sub>)

* **Task Form (T)**, which indicates how to formulate the instructions for apecific tasks. The task forms can be one of the following items:
  * _Pointwise Recommendation_ (T<sub>0</sub>)
  * _Pairwise Recommendation_ (T<sub>1</sub>)
  * _Matching_ (T<sub>2</sub>)
  * _Reranking_ (T<sub>3</sub>)

All of the mentioned aspects for instruction creation are summarized in the following figure:

<p style="text-align:center;"><img src="/assets/img/weekly-review/instructRec_instruction_aspects.png" alt="Instructions Aspects" width="500" height="300"></p>

By having different combinations of the three aspects, about 36 (3 $$\times$$ 3 $$\times$$ 4)  instruction templates are created. Moreover, several techniques are employed to increase the diversity of instruction templates which are explained later. The following figure representes serveral instruction examples, to make it clear that how instruction templates are instantiated (color coding: red=preference, green=intentions, blue=task): 

<p style="text-align:center;"><img src="/assets/img/weekly-review/instructRec_instruction_examples.png" alt="Instructions Aspects" width="800" height="150"></p>

Now, the question is how to fill each aspect placeholder in the instruction example? In other words, how user preferences (red color), user intentions (green color), and task forms (blue color) should be filled? The main strategy for doing so is to automatically generate the prompt by leveraging a strong teacher-LLM (e.g., GPT-3) to be able to generate personaliezd prompt for each user. So, in what follows, we provide more details about how the three aspects--i.e., preference, intention, and task form--can be extracted and annotated to be injected into the instruction templates.

* **Preference Annotation:** Different strategies are used for annotating different preference levels:
  * **Implicit Preference (P<sub>1</sub>):** The item titles are used as representations to create users' historical interactions. After creating the historical interation, the following instruction template is filled: ````The user has previously purchased the following items: {USER_INTERACTION_HISTORY}.````
  * **Explicit Preference (P<sub>2</sub>):** In this case, since most of the datasets do not contain users' explicit preferences, the teacher-LLM is asked to generate explicit expressions of user preferences based on the historical interactions. For example, the following image represents how the teacher-LLM is asked to generate users' explicit preferences:
<p style="text-align:center;"><img src="/assets/img/weekly-review/instructRec_preference_annotion_explicit.png" alt="Instructions Aspects" width="600" height="200"></p>

* **Intention Annotation:** Similar to the preference annotation process, the intention annotaton strategy is determined based on the intention levels:
  * **Vague Intention (I<sub>1</sub>):** To extract the vague intentions, reviews are considered to be very useful and informative, since they provide valuable evidence about user's personal tastes. So, here, the teacher-LLM is provided by item review (I am not sure if the review of the user is used or all the review for a specific item from other users) and is asked to generate the user's intention. An example is provided in the following figure:
  <p style="text-align:center;"><img src="/assets/img/weekly-review/instructRec_intention_annotion_vague.png" alt="Instructions Aspects" width="600" height="200"></p>


  * **Specific Intention (I<sub>2</sub>):** In this case, the category of items with which user had intertions are considered as their specific intentions. For example, it can be: ````Video Games, PC, Accessories, Gaming Mice````.

* **Task Form Annotation:** The task form annotation is determined by the task type:
  * **Pointwise Recommendation (T<sub>0</sub>):** ````Based on the {USER_RELATED_INFORMATION}, is it likely that the user will interact with {TARGET_ITEM} next?```` The model is supposed to answer yes or no!
  * **Pairwise Recommendation (T<sub>1</sub>):** It is not implemented yet. 
  * **Matching (T<sub>2</sub>):** ````Predict the next possible item.````
  * **Reranking (T<sub>3</sub>):** ````Select one item from the following {CANDIDATES}.````


Moreover, several techniques are employed to increase the diversity of instruction templates, including:
* **Turn the Task Around:** It is based on swaping between the input and output of normal instructions. An example of using this strategy is represented in the following figure:
  <p style="text-align:center;"><img src="/assets/img/weekly-review/instructRec_instruction_diversity_swap.png" alt="Instructions Aspects" width="600" height="300"></p>

* **Enforcing the Relatedness between Preference and Intention:** This strategy is based on the hypothesis that the revealed short-term user intentions and their long-term prefernces are highly related. An example of this strategy is as follows:
  <p style="text-align:center;"><img src="/assets/img/weekly-review/instructRec_instruction_diversity_relatedness.png" alt="Instructions Aspects" width="600" height="500"></p>


For evaluation, they have used two datasets:  Games and CDs from Amazon. Also, as baselines, they have considered BERT4Rec, SASRec, GPT3.5, TEM, and DSSM. The metrics used for evaluation are Hit Ratio (HR) and Normalized Discounted Cumulative Gain (NDGC). According to their results, InstructRec could outperform the baselines in many cases.


[InstructRecPaper]: https://arxiv.org/abs/2305.07001
[InstructRecSum]: /blog/2023/week-19/#recommendation-as-instruction-following-a-large-language-model-empowered-recommendation-approach
