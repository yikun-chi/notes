---
description: by Park et al
---

# Generative Agents: Interactive Simulacra of Human Behavior

Park, J. S., O'Brien, J., Cai, C. J., Morris, M. R., Liang, P., & Bernstein, M. S. (2023, October). Generative agents: Interactive simulacra of human behavior. In Proceedings of the 36th annual acm symposium on user interface software and technology (pp. 1-22).

[Annotated Link ](https://drive.google.com/file/d/1UHkmuVdx4ooWT97dEssGztVFZIkM1dDL/view?usp=drive_link), [Github](https://github.com/joonspk-research/generative_agents)

## Framing Notes&#x20;

Claim for significance: "Believable proxies of human behavior can empower interactive applications ranging from immersive environments to rehearsal spaces for interpersonal communication to prototyping tools. "

## Tech&#x20;

Three components:

* memory stream: A list of memory objects, where each object contains a natural language description, a creation timestamp, and a most recent access timestamp.&#x20;
  * Recency score: An exponential decay function over the number of hours passed. Decay factor is 0.995.&#x20;
  * Importance: rated by AI&#x20;
  * Relevance: Cosine similarity between the memory's embedding vector and the query memory's embedding vector
  * retrieval function $$score = \alpha_{recency}*recency + \alpha_{importance}*importance + \alpha_{relevance}*relevance$$
* reflection:&#x20;
  * Periodically generate reflections (e.g., when the sum of the importance scores for the latest events perceived by the agents exceeds a threshold),&#x20;
  * Feed the LLM the previous 100 memory streams, and ask the LLM what the three most salient high-level questions are that we can answer about the subjects in the statements.&#x20;
  * We then use those questions as input to retrieve relevant memories, and subsequently ask the LLM to extract insights from the retrieved memories.&#x20;
* planning&#x20;
  * Multi-stage planning. First, an initial plan is created by prompting the LLM with the agent's summary description and a summary of their previous day.&#x20;
  * The initial plan is then stored in the memory stream and recursively decomposed to create finer-grained actions.&#x20;

Action&#x20;

* At each timestamp, the agent perceives the world around them, then asks the LLM whether to continue with their existing plan or react.&#x20;
* The context summary (fed to LLM in every interaction with other agents) is generated through two prompts that retrieve memories via queries inquiring about the relationship with the other agent and what the other agent is doing.&#x20;

Evaluation:&#x20;

1. A controlled evaluation to test whether the agents produce believable individual behaviors in isolation&#x20;
2. An end-to-end evaluation where the agents interacted with each other in open-ended ways over two days of game time to understand their stability and emergent social behaviors.&#x20;
