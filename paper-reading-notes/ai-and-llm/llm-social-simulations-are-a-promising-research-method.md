---
description: by Anthis et al
---

# LLM Social Simulations are a Promising Research Method

[Annotated Link ](https://drive.google.com/file/d/1fyfTpd_2baJu9WFvKwBONx-nWjPfBZAk/view?usp=drive_link)

## Abstract&#x20;

Accurate and verifiable large language model (LLM) simulations of human research subjects promise an accessible data source for understanding human behavior and training new AI systems. However, results to date have been limited, and few social scientists have adopted this method. In this position paper, we argue that the promise of LLM social simulations can be achieved by addressing five tractable challenges. We ground our argument in a review of empirical comparisons between LLMs and human research subjects, commentaries on the topic, and related work. We identify promising directions, including context-rich prompting and fine-tuning with social science datasets. We believe that LLM social simulations can already be used for pilot and exploratory studies, and more widespread use may soon be possible with rapidly advancing LLM capabilities. Researchers should prioritize developing conceptual models and iterative evaluations to make the best use of new AI systems.

## Quick Notes

This paper provides a great overview of the challenges and current landscape (as of 2025) of LLM agent social simulations. It contains useful references to many works (check out table A1 and A2).&#x20;

## Useful References&#x20;

LLM's ability to reproduce experiments&#x20;

* [Hewitt et al. 2024:](https://samim.io/dl/Predicting%20results%20of%20social%20science%20experiments%20using%20large%20language%20models.pdf) the largest test of sims to date,  &#x20;spanned 70 preregistered and U.S.-representative experiments alongside an archive of replication studies.  &#x20;With a straightforward prompting technique, GPT-4  &#x20;predicted 91% of the variation in average treatment  &#x20;effects when adjusting for measurement error.
* [Binz et al. 2025](https://www.nature.com/articles/s41586-025-09215-4): fine-tuned Llama-3.1-70B on data  &#x20;from 160 human subjects experiments, using this simulator model to outperform existing cognitive models
* [Park et al. 2024a](https://arxiv.org/abs/2411.10109) built 1,052 individual sims, each  &#x20;with an interview transcript from a U.S.-representative  &#x20;sample. The simulator “agents” were able to predict  &#x20;participants’ survey responses 85% as well as did the  &#x20;participants’ responses two weeks before—given the  &#x20;issue of test-retest variation in human subjects data

Concept of evaluation science (i.e., LLM needs more complex evaluation methods): [Weidinger et al. 2025](https://arxiv.org/abs/2503.05336)

[Perception of Sentient AI and AI welfare ](https://arxiv.org/abs/2407.08867)

## Five Challenges of LLM Diversity&#x20;

Diversity / Homogeneity problem: The extent to which a simulation matches the variation of the human population being simulated.&#x20;

* [Gao et al. 2025](https://arxiv.org/abs/2410.19599) LLM failed reproduce human distribution&#x20;
* [Bisbee et al., 2024](https://www.cambridge.org/core/journals/political-analysis/article/synthetic-replacements-for-human-survey-data-the-perils-of-large-language-models/B92267DC26195C7F36E63EA04A47D2FE) LLM produced narrower distributions&#x20;

Bias: Systematic inaccuracies in the representation of particular social groups&#x20;

Sycophancy: The tendency to generate outputs that are excessively optimized for positive feedback from the user&#x20;

Alienness: The tendency of LLMs to superficially match human behavior but operate with non-humanlike mechanism

* [Petrov et al., 2024](https://arxiv.org/abs/2405.07248): LLM personality responses have item level problems&#x20;

Generalization: Simulations need to maintain accuracy in contexts outside the distribution of current scientific knowledge

## Promising Directions

Prompting&#x20;

* Explicit demographics: not idea
* Implicit demographics:&#x20;
  * [Toubia et al., 2025](https://arxiv.org/abs/2505.17479) provides social media&#x20;
  * Park et al used interview data&#x20;
  * (This is a good section to revisit for the GPS project)
* Distribution Elicitation: currently not mature enough&#x20;

Steering Vectors

Token sampling&#x20;

Traning and Tuning&#x20;

## Long-term Directions

Conceptual models:

* [Holtzman et al. 2023](https://arxiv.org/abs/2308.00189)

Iterative evaluation&#x20;







