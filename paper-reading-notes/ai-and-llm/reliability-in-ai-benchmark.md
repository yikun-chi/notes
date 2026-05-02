---
description: Notes from Stanford CS321M
---

# Reliability in AI Benchmark

## Learning Objectives

1. Explain why reliability is non-negotiable in measurement and why validity cannot function without reliability&#x20;
2. Define the general form of reliability and identify its presence across reliability types&#x20;
3. apply the correct form of reliability by identifying the signal and unit of measurement&#x20;
4. Explain the relationship between item and test information and reliability
5. Decompose variance with Generalizability Theory for a crossed $$I \times M \times J$$ benchmark&#x20;
6. Understand the G-coefficients $$Ep^2$$ and dependeability $$\Theta$$ and distinguish relative vs. absolute decision&#x20;



## Motivation&#x20;

Why reliability?&#x20;

* without quantify noise, point estimate comparison is somewhat useless.&#x20;

Traditional ML Evaluation

* Paradigm: running deterministic code&#x20;
* Data: Fixed test sets with known ground truth&#x20;
* Metrics: F1, accuracy, Perplexity&#x20;
* Assumption: The score is a fixed property of the model-data pair &#x20;

AI Behavioral Evaluation (benchmark)

* Paradigm: Observing an experiment&#x20;
* Data: Prompt samples, stochastic LLMs, subjective rubrics&#x20;
* Metrics: Reliability, Generalizability, Conditional Precision&#x20;
* Assumption: The core is an estimate containing measurement noise&#x20;
  * We are estimating a latent variable from observed behavior

reliability&#x20;

* senseable model ranking
* generalize claims&#x20;
* bound validity&#x20;
* justify cost of additional judgment / benchmark&#x20;
* cap correlation (you cannot correlate your benchmark with anything external above $$\sqrt{\rho}$$

## Basic Reliability&#x20;

At the core, reliability is defined as&#x20;

$$
\rho = \frac{signal}{signal + noise}
$$

Generally in variance term

* signal is the variance across objects of measurements (models)&#x20;
* noise is the variance across items, judges, seeds, and other stuff&#x20;

The roadmap of reliability

* We start with observed data (e.g., benchmark response matrix) $$Y$$ which contains signal and (un)structured noise&#x20;
* We can analyze it through&#x20;
  * internal consistency (within scale/ranking)&#x20;
    * signal is the shared variance&#x20;
    * unit (where co-variance comes from) is across items&#x20;
  * agreement / IRR&#x20;
    * signal is agreement beyond chance&#x20;
    * unit is across raters&#x20;
  * latent structure&#x20;
    * signal is latent construct&#x20;
    * unit is across abilities&#x20;
* We have then overall global reliability (e.g., test-retest) and conditional reliability (e.g., varies with abilities)&#x20;
* Alternative option: G-theory, where signal is the object of measurement, and unit is all units of variation&#x20;

## Classical Test Theory (internal consistency)&#x20;

Basic formula $$X = T + E$$ where $$X$$ is what we observed, $$T$$ is the signal we want to observe and $$E$$ is the noise.&#x20;

Axioms:&#x20;

1. $$X = T + E$$
2. $$E[E] = 0$$
3. $$Cov(T,E)=0$$

As a result, we have&#x20;

$$
\begin{align*} \sigma_X^2 &= \sigma_T^2+\sigma_E^2 \\ \rho_{XX'}& = \frac{\sigma_T^2}{\sigma_X^2}=\frac{\Sigma_T^2}{\Sigma_T^2 + \Sigma_E^2}\end{align*}
$$

Essentially,&#x20;

* reliability is defined as proportion of observed-score variance that is the true-score variance&#x20;
* between 0 to 1
* we can use $$\sigma_X \sqrt{1-\rho}$$ as the basic confidence interval around a score

Classical test theory have three level of assumptions&#x20;

1. Parallel: equal loadings, equal error variance&#x20;
   1. $$X_k = T + E_k, Var(E_k)=\sigma_E^2$$
   2. strong, and rarely true&#x20;
2. $$\tau$$-equivalent: equal loadings, unequal error variance&#x20;
   1. $$X_k = T + E_k, Var(E_k) \text{ differs}$$
   2. Cronbach's alpha lower bound, but still a terrible idea.&#x20;
      1. loadings are normally unequal&#x20;
      2. correlated errors lead to positively biased&#x20;
      3. can't interpret multidimensional scale&#x20;
      4. Equivalent to KR-20 under $$\tau$$-equiv
      5. Cronbach himself doesn't recommend it (2004)
      6. better alternative&#x20;
         1. McDonald's $$\omega_t$$
         2. proportion due to general factor (hierarchical) $$\omega_h$$
         3. Greatest Lower Bound (GLB)&#x20;
         4. Generalizability coefficient $$E\rho^2$$
3. Congeneric: Items load differently on $$T$$
   1. $$X_k = \lambda_k T + E_k$$
   2. Need $$\omega$$, G-theory or CFA

## &#x20;Agreement / IRR&#x20;

### ICC

Intraclass Correlation (Variance decomposition) by collect repeated measurement&#x20;

$$
\rho = \frac{\sigma^2_{\text{between}}}{\sigma^2_{\text{between}} + \sigma^2_{\text{within}}}
$$

ICC of items x judges (I X J) is estiamted from a two-way ANOVA model&#x20;

$$
X_{ij}=\mu+\alpha_i+\beta_j+(\alpha\beta)_{ij}+\epsilon_{ij}
$$

where $$\alpha_i$$ is the item effect (the signal), $$\beta_j$$ is the judge effect (rater bias) and the $$(\alpha \beta)_{ij}$$ is the interaction.&#x20;

Choosing ICC  #TODO (Shrout & Fleiss) / ICC Flowchart&#x20;

ICC assumptions and issues

* Continuous scores, roughly symmetric residuals&#x20;
* Raters sampled from a population (ICC 1, 2); constatn rater set (ICC 3)&#x20;
* Restricted range shrinks $$\sigma_{signal}^2$$ and deflates ICC - common when items are too easy for top models&#x20;
* Population-dependent; an ICC is a property of the text x population, not the test alone&#x20;
* Does not distinguish bias from nose&#x20;

### Krippendorff's alpha&#x20;

$$
\alpha_{krip} = 1 - \frac{\text{error}}{\text{expected error}}
$$

it arises from the $$\kappa/\alpha$$ family (chance-correlated agreements)&#x20;

| Score type          | Design             | Typical Choice            | (Often) better    |
| ------------------- | ------------------ | ------------------------- | ----------------- |
| Continuous / Likert | 2+ raters, crossed | ICC(2,k) or $$E\rho^2$$   | $$E\rho^2(REML)$$ |
| Ordinal             | 2 raters           | Weighted kappa            | Krippendorff      |
| Nominal             | 2 raters           | Cohen's kappa             | Gwet's AC1        |
| Nominal             | ≥ 3 raters         | Fleiss's kappa            | Gwet's AC1        |
| Any                 | Any                | Krippendorff's $$\alpha$$ | Krippendorff      |

The pitfall of Kappa&#x20;

* prevalence: useless for imbalanced data (when base rate is one sided, kappa is artificially low)
  * use Gwet's AC1 or ICC on ordinal collapse&#x20;
* bias : if raters have very different base rates but agree on patterns, which rises kappa since kappa confounds marginal asymmetry with agreement structure&#x20;
  * report marginals separately or use weighted kappa or alpha&#x20;

Core takeaway

* For continuous/ordinal score, abandon agreement metrics entirely for ICC or G-theory&#x20;

## Latent Structure&#x20;

IRT Conditional Reliability&#x20;

$$
\rho(\theta)=\frac{J(\theta)}{J(\theta) + 1}
$$

where $$\rho(\theta)$$ is the conditional reliability, which is based on the fisher information. $$J(\theta)$$ has inverse relationship to variance (#todo, connect to previous IRT lecture and revisit, april 20th lecture )

Test information&#x20;

* $$I(\theta)=\sum_{i} I_i(\theta)$$
* $$CSEM(\theta) = \frac{1}{\sqrt{I(\theta)}}$$ (conditional standard error)
* $$\rho(\theta) = 1 - \frac{1}{I(\theta)(\sigma_\theta^2)}=1 - \frac{CSEM^2(\theta)}{\sigma_\theta^@}$$

So overall, it tells you that your reliability conditioned on item difficulty&#x20;

## Generalizability Theory

Setup&#x20;

* Objective of measurement (M): what we want to rank (e.g., AI models)&#x20;
* Facets (I, J, ...): conditions of observation / source of structured noise (items, judges, prompts)
  * must be repeated obs&#x20;
* Universe of admissible observations: the set of (m, i, j, ...) we would be willing to treat as equivalent or of "similar construction" (constraints)
* Universe score $$\mu_M$$: expected score of model M over the universe, which is the G-theory analog of T.  (signal)&#x20;
* G-study: decompose variance from a smaple of the universe&#x20;
* D-study (next time): plug variance components into design decisions&#x20;

Advantage&#x20;

* models multiple noise facets simultaneously&#x20;
* accounts for interactions and nested and crossed designs&#x20;
* separates relative decisions like ranking order of variance from absolute decisions like the actual scores of variance&#x20;

Relative vs. absolute error&#x20;

* relative ranking (generalizability coefficient)
  * error variance $$\sigma_s^2$$= only facets that interact with model&#x20;
  * coefficient: $$E\rho^2=\frac{\sigma_M^2}{\sigma_M^2 + \sigma_s^2}$$
  * answers: is model A better than model B&#x20;
* absolute criterion (Dependability Index)
  * error variance $$\sigma_\delta =$$ all facets except $$\sigma_M^2$$
  * $$\Phi=\frac{\sigma_M^2}{\sigma_M^2+\sigma_\delta^2}$$
  * answers: does model A exceed 80%
* rule of thumb:&#x20;
  * $$\Phi <= E\rho^2$$, if $$\sigma_I^2$$ dominates, they can diverse sharply
  * hence, dependability is never better than generalizability\
    $$E\rho^2 = \frac{sigma}$$



$$CSEM(\theta)=\frac{1}{\sqrt{I(\theta)}$$
