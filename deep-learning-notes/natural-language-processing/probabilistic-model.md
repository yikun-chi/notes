---
description: >-
  https://www.coursera.org/learn/probabilistic-models-in-nlp?specialization=natural-language-processing
---

# Probabilistic Model

## Autocorrect with Minimum Edit Distance&#x20;

### Key Idea

1. Identify a misspelled word&#x20;
2. Find strings $$n$$edit distance away, normally 1 - 3
3. Filter candidates (only restrict to correclty spelled word)&#x20;
4. Calculate wor probability $$p(w) = \frac{count(w)}{V}$$ where $$V$$is the size of corpus. A more advanced version would be to track two words co-occurring and use the word before misspelled word to identify the correct autocorrect.&#x20;

### Find Minimum Edit Distance with Dynamic Programming&#x20;

Given word $$x$$ (source) and $$y$$ (target), construct a matrix where the columns are alphabets of word $$x$$ and rows are alphabets of $$y$$. Both should include a blank / starting line character. Cell $$i,j$$ means the minimum edit distance that goes from $$w_x[:i]$$to $$w_y[:j]$$. Notice here $$w_x[:j]$$ refers to all the characters include $$i$$. Therefore, the minimum edit distance problem can be decomposed into&#x20;

$$
\begin{equation*}
        D[i,j] = min
            \begin{cases}
                D[i-1,j] + del\_cost \\
                D[i,j-1] + ins\_cost \\
                D[i-1, j-1] +
                    \begin{cases}
                        rep\_cost & w_x[i] \neq w_y[j] \\
                        0 & else
                    \end{cases}
            \end{cases}
    \end{equation*}
$$

We can look at example below transforming from "play" to "stay" with insert and delete cost 1 and replace cost of 2.&#x20;

$$
\begin{align*}
\begin{matrix}
   & \# & s & t & a & y \\
\# & 0 & 1 & 2 & 3 & 4 \\ 
p & 1 & 2 & 3 & 4 & 5 \\
l & 2 & 3 & 4 & 5 & 6 \\
a & 3 & 4 & 5 & 4 & 5 \\
y & 4 & 5 & 6 & 5 & 4 \\
\end{matrix}
\end{align*}
$$

We can pick cell (a,s) as an example, which means we want to know the edit distance from "pla" to "s":&#x20;

1. If we are at cell (l, s), we know the edit distance from "pl" to "s". So we can do a deletion to go from "pla" to "pl", then use the knowledge to go from "pl" to "s"/&#x20;
2. If we are at cell (a, #), we know the edit distance from "pla" to "#". So we can do an insertion of "s" after going from "pla" to "#"
3. If we are at cell (l, #), we know the edit distance from "pl" to "#". Since the target is "pla" to "s", and "a" is not same as "s", we have to do a replacement. Imagine if we are going from "pls" to "s", then it would be the same cost as going from "pl" to "#".&#x20;

**Backtracing**&#x20;

As we construct the table, we need to keep a pointer in each cell on how I get to the cell.&#x20;

## Part of Speech Tagging and Hidden Markov Model&#x20;

### Dealing with Unknown Words When Processing a Document&#x20;

We can replace unknown words with different unknown tokens, such as "--unk\_digit--", "--unk\_punct--" and etc.  [Notebook practice ](https://drive.google.com/file/d/1FHZ\_SxK58imWGYTZzs5nalnNteofFWZY/view?usp=share\_link)

### Hidden Markov Model as PoS Transition&#x20;

Hidden nodes: Part of Speech, e.g.: verb, noun&#x20;

Observable nodes: Actual words, e.g.: like, use&#x20;

### Smoothing in Calculating Transition Probabilities&#x20;

Original transition probability: ($$t_i$$ is the tag at location $$i$$ )&#x20;

$$
\begin{align*}
        P(t_i|t_{i-1}) = \frac{Count(t_{i-1}, t_i)}{\sum_{j=1}^N Count(t_{i-1}, t_j)}
    \end{align*}
$$

We can add smoothing to deal with cases of 0, which can cause 1) division by 0 problem in probability calculation and 2) probability of 0, which doesn't generalize well.  So we calculate transition probability as:&#x20;

$$
\begin{align*}
        P(t_i|t_{i-1}) = \frac{Count(t_{i-1}, t_i) + \epsilon}{\sum_{j=1}^N Count(t_{i-1}, t_j) + N * \epsilon}
    \end{align*}
$$

### Smoothing in Calculating Emission Probabilities&#x20;

Following the same principle, we can calcuate emission probabilities as&#x20;

$$
\begin{align*}
p(w_i|t_i) = \frac{Count(t_i, w_i) + \epsilon}{\sum_{j=1}^V Count(t_i, w_j) + N * \epsilon}
\end{align*}
$$

[Deepdive into Hidden Markov Models](../../statistics-method-notes/statistical-test-and-tools/hidden-markov-models.md)&#x20;

## Code&#x20;

### Counter&#x20;

```python
// Some code
Counter('abracadabra').most_common (3)
```

## Completed Notebook

[Autocorrection](https://drive.google.com/file/d/1Ob6shW\_Tv2-j\_BF4vn0HDRvQcWcaEjaS/view?usp=share\_link)

* Dynamic Programming&#x20;
* String Edit (deletion, insertion, replacement) as List Comprehension (very interesting)&#x20;
* Using Counter object

