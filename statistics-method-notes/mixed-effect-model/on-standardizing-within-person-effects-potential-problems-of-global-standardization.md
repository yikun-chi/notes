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

### Set-up

Given variable $$X$$ and $$Y$$, the true within-person correlation is&#x20;

$$
\begin{align*}
\rho_{w,i}  &=\frac{E_i\left[(X_{it}-\mu_{X_i})(Y_{it}-\mu_{Y_i})|i\right]}{\sigma_{X_i}\sigma_{Y_i}} \\
\mu_{\rho w} &= E[\rho_{w,i}]
\end{align*}
$$

* $$\mu_{X_i} \& \mu_{Y_i}$$: Population within-person means
* $$\sigma_{X_i} \&  \sigma_{Y_i}$$: Individual within-person standard deviations
* $$\mu_{\rho w}$$: is the average within-person correlation, generally the true parameter of interests.&#x20;

### Consistent Estimators

Let $$r_{w,i} =\frac{\sum_t(x_{ti}-x_i)(y_{ti}-y_i)}{T*s_{xi}*s_{yi}}$$ be the person-specific correlation estimators using the sample mean and within-person sample standard deviation, then generally we have the following multilevel models:&#x20;

$$
\begin{align*}
r_{w,i} &= \rho_{w,i} + e_{it} \\
\rho_{w,i} &= \mu_{\rho w} + u_{1i}
\end{align*}
$$

So the estimator is a combination of true correlation and error, and true correlation is a combination of population mean and then person-specific variations.&#x20;

### Person-mean centering followed by global standardization formulation

The basic multilevel model (C1) is&#x20;

$$
\begin{align*}
y_{it} &= \gamma_{0i}^{C1} + \gamma_{1i}^{C1}(x_{it}-x_i) + e_{it}^{C1} \\
\gamma_{0i}^{C1} &= \gamma_{00}^{C1} + \gamma_{01}^{C1}x_i + u_{0i}^{C1} \\
\gamma_{1i}^{C1} &= \gamma_{10}^{C1} + u_{1i}^{C1}
\end{align*}
$$

* Without allowing variation to within-person effect $$u_{1i}^{C1}$$, the estimation of within-person effect yielded inflated Type 1 error (false positive).&#x20;
* Including the term when there are no between-person variations of the within-person effect, the model has a higher non-convergence rate.&#x20;
* The person-mean centering is also conducted for the outcome variable implicitly. We can see that if we move the $$\gamma_{0i}^{C1}$$ to the other side.&#x20;

A typical multilevel model with explicitly mean centering (C2): Notice the outcome variable is mean-centered with a sample instead of a latent estimate, and PC annotes as person-specific centering.&#x20;

$$
\begin{align*}
y_{it}^{PC} &= \gamma_{1i}^{C2}*x_{it}^{PC}+e_{it}^{C2} \\
\gamma_{1i}^{C2} &= \gamma_{10}^{C2} + u_{1i}^{C2}\\
y_{it}^{PC} &=y_{it} - y_i\\
x_{it}^{PC} &=x_{it} - y_i\\
\end{align*}
$$

* Empirically, the estimated within-person mean $$\gamma_{10}^{C2}$$ often has high reliability even when the number of time points is as low as 5 or 10.&#x20;

The estimators are then standardized using global standard deviations. We have

* G1 method: $$\hat{\gamma}_{10}^{G1} = \hat{\gamma}_{10}^{C1}*\frac{s_{cx}}{s_{cy}}$$ (using the person-mean centered IV and DVs)
* G2 method: $$\hat{\gamma}_{10}^{G2} = \hat{\gamma}_{10}^{C1}*\frac{s_{cx}}{s_{y}}$$(using the person-mean centered IV and raw DV sd)
* G3 method: $$\hat{\gamma}_{10}^{G3} = \hat{\gamma}_{10}^{C2}*\frac{s_{cx}}{s_{cy}}$$(using the person-mean centered IV and DVs with explicitly mean centering)&#x20;











Estimator $$\gamma_{10}^{C1}$$ can be&#x20;









