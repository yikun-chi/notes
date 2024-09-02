---
description: https://www.coursera.org/learn/probabilistic-models-in-nlp/home/week/2
---

# NLP-C2-W2: PoS Tagging and HMM

## Document Processing&#x20;

### Dealing with Unknown Words When Processing a Document&#x20;

We can replace unknown words with different unknown tokens, such as "--unk\_digit--", "--unk\_punct--" and etc.  [Notebook practice ](https://drive.google.com/file/d/1FHZ\_SxK58imWGYTZzs5nalnNteofFWZY/view?usp=share\_link)

## PoS Transition HMM&#x20;

### Setup: &#x20;

Hidden nodes: Part of Speech, e.g.: verb, noun&#x20;

Observable nodes: Actual words, e.g.: like, use&#x20;

### Smoothing in Calculating Transition Probabilities&#x20;

Original transition probability: ($$t_i$$ is the tag at location $$i$$ )&#x20;

$$
\begin{align*}
        P(t_i|t_{i-1}) = \frac{Count(t_{i-1}, t_i)}{\sum_{j=1}^N Count(t_{i-1}, t_j)}
    \end{align*}
$$

We can add smoothing to deal with cases of 0, which can cause 1) a division by 0 problems in probability calculation and 2) a probability of 0, which doesn't generalize well.  So, we calculate transition probability as follows:&#x20;

$$
\begin{align*}
        P(t_i|t_{i-1}) = \frac{Count(t_{i-1}, t_i) + \epsilon}{\sum_{j=1}^N Count(t_{i-1}, t_j) + N * \epsilon}
    \end{align*}
$$

* $$N$$ is the total number of tags&#x20;

### Smoothing in Calculating Emission Probabilities&#x20;

Following the same principle, we can calculate emission probabilities as&#x20;

$$
\begin{align*}
p(w_i|t_i) = \frac{Count(t_i, w_i) + \epsilon}{\sum_{j=1}^V Count(t_i, w_j) + N * \epsilon}
\end{align*}
$$

* $$N$$ is the total number of words&#x20;

[Deepdive into Hidden Markov Models](../../statistics-method-notes/hidden-markov-models.md)&#x20;

## Code&#x20;

### Counter&#x20;

```python
// Some code
Counter('abracadabra').most_common (3)
```

## Completed Notebook

[Part of Speech Tagging](https://drive.google.com/file/d/1sGzQF5LFhzIoF4Df5mMTFj0gIAr7sxnv/view?usp=share\_link)

* Clear structure in pre-processing and actual modeling&#x20;
* Viterbi algorithm implementation&#x20;

