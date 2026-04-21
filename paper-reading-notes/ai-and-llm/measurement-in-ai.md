---
description: Notes from Stanford CS321M
---

# Measurement in AI

## Basic of Measurement & Item Response Theory

### Response Matrx:&#x20;

Response matrix is a matrix with $$N$$ (number of models) rows and $$M$$(number of benchmark questions) columns, and generally each entry $$Y_{ij}\in \{ 1,0\}$$ denotes whether the model $$i$$ get question $$j$$ correct.&#x20;

### Rasch Model (1PL)&#x20;

Notation: $$\theta$$ describes model capability / ability. $$\beta$$ describes item difficulty, and $$\alpha$$ describes item informativeness&#x20;

$$
P(Y_{ij}=1|\theta_i, \beta_j)=\sigma(\theta_i-\beta_j)=\frac{1}{1+e^{-(\theta_i-\beta_j)}}
$$

So in 1PL model, we say the probability of a model $$i$$ answering question $$j$$ correctly is based on model capability and item difficulty. If $$\theta_i=\beta_j$$, then $$p=0.5$$, and so on.&#x20;

Benefit:&#x20;

* In this framework, two models with the same accuracy are not actually equal if one got harder question right.&#x20;
* coefficients are estimated with standard error&#x20;
* you can make prediction on unseen items&#x20;
* you can derive information function $$I(\theta)=\sum_{j=1}^M P_j(\theta)(1-P_j(\theta))$$ where $$P_j(\theta)=\sigma(\theta-\beta_j)$$. This function will tell you at what range of model capability does the benchmark most informative (peak of the function).&#x20;

**Item characteristic curves (ICC)** is a curve with ability on the x-axis and probability of answering item correctly on the y-axis. So we would have multiple item curves based on the item difficulty.&#x20;

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Example of ICC</p></figcaption></figure>

**Saturation under IRT**: a benchmark is saturated when all item difficulties $$\beta_j$$ are below the frontier model abilities $$\theta_i$$. In this cause the information function is virtually 0. SOTA claim on saturated benchmark is measuring noise at the top of the scale.&#x20;

#### Useful properties under assumption of Rasch model&#x20;

**Sufficiency of sum scores**&#x20;

&#x20;$$S_i=\sum_{j}Y_{ij}$$ is a sufficient statistics for $$\theta_i$$, hence $$P(Y_i|S_i, \theta_i)=P(Y_i|S_i)$$. So knowing which item is correct gives you nothing compare to knowing how many is correct (hence accuracy is enough).&#x20;

**Specific Objectivity**

Model comparisons are item-independent, $$\frac{\textbf{odds}(Y_{ij}=1)}{\textbf{odds}(Y_{kj}=1)}=exp(\theta_i-\theta_k)$$, so comparing the performance of two models are item-agnostic.&#x20;

But in reality, there are many ways in which Rash model / IRT doesn't hold.&#x20;

* IRT assumes $$Y_{ij} \perp Y_{ik} | \theta_i$$, but LLMs have systematic failures.&#x20;
* LLMs don't guess randomly&#x20;
* some benchmark items leak into training data, changing effective $$\beta_j$$ without the item changing&#x20;



### Two-Parameter Logistic (2PL)

$$
P(Y_{ij}=1)=\sigma(\alpha_j(\theta_i-\beta_j))
$$

where $$\alpha_j$$ is the discrimination of item $$j$$. High $$\alpha_j$$ means steep ICC, sharply distinguishes ability, and low $$\alpha_j$$ means flatter ICC, less informative.&#x20;

### Three-Parameter Logistic (3PL)

$$
P(Y_{ij}=1)=c_j + (1-c_j)\sigma(\alpha_j(\theta_i-\beta_j))
$$

where $$c_j\in [0,1]$$ is the guessing parameter. Higher $$c_j$$ means easier to guess, generally is $$\frac{1}{\textbf{number of item}}$$



## Comparison / Rank Model&#x20;

### Bradley - Terry Model

$$
P(\textbf{model i beats model j})=\sigma(\theta_i-\theta_j)
$$

here $$\theta$$ is the strength of the model. Overall this has the same form as Rasch, but different interpretation since there is no item response, it is simply model comparison.&#x20;

### Elo Rating System&#x20;

$$
R_i^{new}=R_i + K(S_i-E_i)
$$

where $$S_i \in \{0, 0.5, 1\}$$ denotes the outcome, and $$E_i=\sigma(\frac{(R_j-R_i) ln(10)}{400})$$ and $$K$$ is a learning rate parameter.&#x20;



## Scaling&#x20;

Three axis of scaling law \[TODO: add from lecture]

Kaplan et al. (2020), a core obs is loss follows a power law: $$L(x)\approx(\frac{x_0}{x})^\alpha$$ where $$x$$ is the resource, $$x_0$$ is a fitted constant and L is the cross entropy loss.&#x20;

* The Chinchilla correction: Kaplan's model was undertrained.
* This only applies to loss, it does NOT translate to downstream task performances&#x20;

Observational scaling (Ruan, Maddison & Hashimoto 2024)

* Build scaling law from \~100 existing public models Model families differ in efficiency, but share a low-dimensional capability space that predicts performance across all families&#x20;

Test-time scaling (

