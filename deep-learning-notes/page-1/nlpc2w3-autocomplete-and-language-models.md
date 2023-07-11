---
description: https://www.coursera.org/learn/probabilistic-models-in-nlp/home/week/3
---

# Autocomplete and Language Models

## N-gram and Sequence Probabilities

N-gram $$(w_1, ..., w_n)$$ probability is defined as conditional probability&#x20;

$$
p(w_n|w_1,...,w_{n-1},w)= \frac{Count(w_1,...,w_{n-1},w_n)}{Count(w_1,...,w_{n-1})}
$$

Subsequently, we can define the sequence bigram probability as (N-gram follows the same structure)&#x20;

$$
p(w_1,...,w_n)=p(w_1)*p(w_2|w_1)*...*p(w_{n-1}|w_n)
$$

In general, add start token _\<s>_ until there is enough length to start and 1 end token _\</s>._ This fixes:&#x20;

1. Getting the correct count for N-gram probability calculation&#x20;
2. Make the sum of probability of all possible sentence with all possible lengths to be 1 instead for each certain length, the probability sum of all possible sentence at that length to be 1 (enable generalization)

### Operationalization&#x20;

Count Matrix:

* row: unique corpus (N-1)-grams&#x20;
* columns: unique corpus words
* cell value: the count of N-gram (row, column)&#x20;

Probability matrix

* Divide each cell by its row sum&#x20;

Other consideration&#x20;

* use log to convert multiplication to sumation&#x20;
* use _\<UNK>_ to mark out of vocabulary word&#x20;
  * vocabulary can be constructed through
    * min word frequency&#x20;
    * max vocab size betermined by frequency&#x20;
    * expertise / requirement (e.g.: no swear word)&#x20;

### Smoothing&#x20;

* Add-one smoothing ([Laplacian smoothing](../natural-language-processing/classification-methods-and-vector-space.md#naive-bayes-frequency-ratio-representation)): When calculating probability, add 1 to the numerator count and add $$|V|$$ to the denominator&#x20;
* Add-k smoothing: add $$k$$ to the numerator and $$k * |V|$$ to the denominator
* More advanced: Kneser-Ney smoothing, Good-Turing smoothing

### Backoff&#x20;

If an N gram is missing, use N-i gram until a non-zero probability is reached&#x20;

* "stupid" backoff: multiply probability by a constant to discount&#x20;

### Interpolation&#x20;

Define a N-gram probability as the weighted probability of all order of N gram, e.g.:&#x20;

$$
\hat{p}(w_3|w_1,w_2)=\lambda_1*p(w_3|w_1,w_2)+\lambda_2*p(w_2|w_1)+\lambda_3*p(w_1)
$$

note all lambda should sums to 1





## Language Model Evaluation - Perplexity&#x20;

Train-Validation-Test split are constructed by splitting continuous text (e.g.: article) or by random short sequences (e.g.: part of sentences).&#x20;

Perplexity is defined as the probability of all sentences (multiplied together) raised to the -1 over number of words (not including start token, but include end token)

$$
perplexity = p(s_1,...,s_m)^{-1/m}
$$

A good model has low perplexity, such as 60-20 for English or 5.3-5.9 for log perplexity



## Completed Notebook

[Autocomplete](https://drive.google.com/file/d/14HihsV09Nugg\_uNJstFVV9Fmnw4b-p8P/view?usp=share\_link)

* Calculating N-gram probabilities&#x20;
