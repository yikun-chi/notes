# Hidden Markov Models

Materials are collected from&#x20;

* [Coursera NLP course](https://www.coursera.org/learn/probabilistic-models-in-nlp/)&#x20;

## Objective 2: Determining hidden states given sequence and model

Obtaining the maximum a posteriori probability estimate of the most likely sequence of hidden states given a sequence of observed states

### Viterbi Algorithm&#x20;

#### Components:&#x20;

* Transition matrix $$A$$ and emission matrix $$B$$ of Hidden Markov Model&#x20;
* Auxiliary matrix $$C_{n \times k}$$ which tracks the intermediate optional probability. $$n$$ is the number of hidden states and $$k$$ is the number of words in observed states (e.g.: a text sequence)&#x20;
* Auxiliary matrix $$D_{n \times k}$$, which tracks the indices of visited states

#### Initialization:&#x20;

1. Fill the first column of $$C$$( the probability from start sate to the first word with each hidden state) with: $$c_{i,1} = \pi_i * b_{i, cindex(w_1)}$$
   * $$\pi_i$$: The transition probability from start state $$\pi$$ to hidden state $$i$$
   * $$b_{i, cindex(w_1)}$$: The emission probability of word 1 (which has column index $$cindex(w_1)$$ in emission matrix) at hidden state $$i$$.&#x20;
2. Fill the first column of $$D$$ with 0. This is becase there is no preceding part of speech tags that we traversed from. &#x20;

#### Forward Pass&#x20;

Populate the rest of $$C$$ and $$D$$ matrix column by column with the following formula:&#x20;

$$
\begin{align*}
        & c_{i,j} = \max_k c_{k, j-1} * a_{k,i} * b_{i, cindex(w_j)}\\
        & d_{i.j} = \argmax_k  c_{k, j-1} * a_{k,i} * b_{i, cindex(w_j)}\\
    \end{align*}
$$

* $$a_{k,i}$$ is the transition probability from state $$k$$ to state $$i$$

Logic: Look at $$c_{i,j}$$, we want to find the optimal probability for the word sequence up to word $$j$$ where the word $$j$$ speech tag is $$t_i$$. This is by iteratively looking over all possible previous $$c_{k, j-1}$$, i.e.: the tag optiosn ending at word $$j-1$$. For each optimal probability $$c_{k, j-1}$$,we then multiply the probability of transition from $$t_k$$ to $$t_i$$, hence the $$a_{k,i}$$, and the emission probability of word $$j$$ at $$t_i$$, hence $$b_{i, cindex(w_j)}$$. For$$D$$matrix, we want to track the argmax, which is which previous hidden states did we decide to travel from. &#x20;

#### Backward Pass

1. Get $$s = \argmax_i c_{i, K}$$. The probability at index $$s$$ is the posterior probability estimate of the sequence.&#x20;
2. We go to column $$K$$ of $$D$$, and find the index at row $$s$$, i.e.: $$tag_{K-1} = d_{s, K}$$
3. Go to the left next column ($$K-1$$), and find the index at row $$tag_{K-1}$$, i.e.: $$tag_{K-2} = d_{tag_{K-1}, k-1}$$

#### Considerations: Log Probability&#x20;

Because we are multiplying many small numbers, we should again consider using log probability, hence&#x20;

$$
\begin{align*}
        \log(C_{i,j}) = \max_k \log(C_{k, j-1}) + \log(a_{k,i}) + \log(b_{i, cindex(w_j)})
    \end{align*}
$$







