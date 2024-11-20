---
description: By Lijuan Wang, Qian Zhang, Scott E Maxwell and C S Bergeman
---

# On Standardizing within-person effects: Potential problems of global standardization

## Abstract

Person-mean centering has been recommended for disaggregating between-person and within-person effects when modeling time-varying predictors. Multilevel modeling textbooks recommended global standardization for standardizing fixed effects. An aim of this study is to evaluate whether and when person-mean centering followed by global standardization can accurately estimate fixed-effects within-person relations (the estimand of interest in this study) in multilevel modeling. We analytically derived that global standardization generally yields inconsistent (asymptotically biased) estimates for the estimand when between-person differences in within-person standard deviations exist and the average within-person relation is nonzero. Alternatively, a person-mean-SD standardization (P-S) approach yields consistent estimates. Our simulation results further revealed (1) how misleading the results from global standardization were under various circumstances and (2) the P-S approach had accurate estimates and satisfactory coverage rates of fixed-effects within-person relations when the number of occasions is 30 or more (in many conditions, performance was satisfactory with 10 or 20 occasions). A daily diary data example, focused on emotional complexity, was used to empirically illustrate the approaches. Researchers should choose standardization approaches based on theoretical considerations and should clearly describe the purpose and procedure of standardization in research articles.

## Primer

Issues about raw units&#x20;

* The interpretation of a raw within-person coefficient depends on the scale of two time-varying variables (so not meaningful if the scale itself is not meaningful)
* Raw coefficients are generally not good effect size measures because they are not scale/unit invariant
* Raw coefficient usually is less useful for comparing the strengths of within-person relations across different pairs of time-varying variables.&#x20;

## Formulation&#x20;

Given variable $$X$$ and $$Y$$, the true within-person correlation is&#x20;

$$
\begin{align*}
\rho_{w,i}  &=\frac{E_i\left[(X_{it}-\mu_{X_i})(Y_{it}-\mu_{Y_i})|i\right]}{\sigma_{X_i}\sigma_{Y_i}} \\
\mu_{\rho w} &= E[\rho_{w,i}]
\end{align*}
$$

* $$\mu_{X_i} \& \mu_{Y_i}$$: Population within-person means
* $$\sigma_{X_i} \&  \sigma_{Y_i}$$: Individual within-person standard deviations
*   $$\mu_{\rho w}$$: is the average within-person correlation, generally the true parameter of interests.&#x20;







