---
description: by He et al
---

# Impatient Users Confuse AI Agents

[Link](https://aclanthology.org/2026.acl-long.1743/)

## Broad Idea

Define user persona as the combination of a personality traits $$P_t$$ and extrinsic user attributes $$P_a$$

* $$P_t$$ is a transormation from trait criteria $$C$$ (discrete trait dimension) into a continuous representation.&#x20;
* So imagine $$C=\{c_1,c_2,...,c_k\}$$ where $$c_i$$ is a discrete trait like impatience with three levels (low, mid, high). Then mapping function $$F: C^k \rightarrow \mathbb{R}^d$$ maps the discrete trait to personality vector $$P_t$$
* Attribute vector $$P_a$$ is constructed from phrases describing a user's immutable attributes such as age, occupation.&#x20;

