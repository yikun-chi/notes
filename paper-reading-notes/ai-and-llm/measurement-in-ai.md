---
description: Notes from Stanford CS321M
---

# Measurement in AI

## Scaling&#x20;

Three axis of scaling law \[TODO: add from lecture]

Kaplan et al. (2020), a core obs is loss follows a power law: $$L(x)\approx(\frac{x_0}{x})^\alpha$$ where $$x$$ is the resource, $$x_0$$ is a fitted constant and L is the cross entropy loss.&#x20;

* The Chinchilla correction: Kaplan's model was undertrained.
* This only applies to loss, it does NOT translate to downstream task performances&#x20;

Observational scaling (Ruan, Maddison & Hashimoto 2024)

* Build scaling law from \~100 existing public models Model families differ in efficiency, but share a low-dimensional capability space that predicts performance across all families&#x20;

Test-time scaling (

