---
description: 'Visser & Speekenbrink: Sec 2.1, 2.2'
---

# Mixture Model Setup

## Definition

Mixture model models $y$ as a series of random independent draw from different states (distributions). A mixture model with $$N$$ states is defined by

* The state densities $$f_i$$, $$i = 1,..., N$$
* The state probabilities $$\pi_i$$, $$i = 1,...,N$$

The actual outcome is modeled as

$$
f(Y=y) = \sum_{i=1}^N\pi_i f_i(Y=y)
$$

## Parameters

$$\theta_i$$: parameters of the component distributions $$f_i$$

$$\theta_{obs}$$: parameters of the observation densities, which contains all $$\theta_i$$ for $$i=1,...,N$$

$$\theta_{pr}$$: parameters for the mixing proportions.

$$z$$: covariates. Currently ignored since we typically can model the mixture model with remaining parameters. But technically we can model component distribution as $$f_i(y_t|\theta_i, z_t)$$ and mixing proportions as $$p(S_t = i|Z_t, \theta_{pr})$$. This allow distributions and proportions vary across observations. Detail discussed later.

## Likelihood

$$
\begin{equation*} L(\theta|y_{1:T}) = \prod_{t=1}^T \sum_{i=1}^N \pi_i f_i(y_t|\theta_i) \end{equation*}
$$

This likelihood can be optimized by using EM algorithm or direct maximization. During actual maximization process we often choose to maximize log likelihood.

## Posterior Probabilities

Posterior probability is the probability that the state at observation $$t$$ given observation is $$y_t$$

$$
\begin{equation*} p(S_t = i | y_t) = \frac{\pi_i f(y_t|S_t=i)}{\sum_{j=1}^N \pi_j f(y_t|S_t=j)} \end{equation*}
$$
