---
description: https://www.coursera.org/learn/probabilistic-models-in-nlp/home/week/4
---

# Word Embeddings with Neural Networks

## Common Embedding method

word2vec (Google 2013)&#x20;

* Continuous bag-of-words (CBOW): Use context words around the center word to predict center word
* Continuous skip-gram / Skip-gram with negative sampling (SGNS)&#x20;

Global Vectors (GloVe) (Stanford, 2014(&#x20;

fast Text(Facebook, 2016)&#x20;

* Supports out-of-vocabulary words&#x20;
* Can average word embeddings to get embedding for sentences&#x20;

Bert (Google, 2018)&#x20;

ELMo( Allen Institute for AI, 2018)&#x20;

GPT-2 (OpenAI, 2018)&#x20;

## CBOW Architecture&#x20;

Initialization: one-hot vector for word embedding&#x20;

Model Architecture:&#x20;

* Input layer:&#x20;
  * Takes in the average of all context words embedding (size V)&#x20;
  * ReLU activation
  * Recall that input is stacked vertically as $$V \times n$$ where $$n$$ is the number of input.
* Hidden layer:&#x20;
  * 1 layer, size is the embedding dimensions&#x20;
  * softmax activation&#x20;
* Output layer :&#x20;
  * The probability distribution over vocabulary. Pick the arg-max to be the actual predicted word&#x20;
* Cost function:&#x20;
  * cross entropy loss b/t softmax output and one-hot vector of actual word&#x20;

Obtain word embedding:&#x20;

1. The weight matrix of input layer is $$d \times V$$ where $$d$$ is the embedding dimension. So each column can be seen as the embedding to the corresponding word&#x20;
2. The weight matrix of hidden layer is $$V \times d$$, so we can use each row as the corresponding word vector
3. Take the average of option 1 and 2

Model Evaluation&#x20;

* Intrinsic evaluation: test relationships between words&#x20;
  * analogy
    * semantic : e.g.: "France" is to "Paris" as "Italy" is to ?&#x20;
    * syntactic : e.g.: "Seen" is to "saw" as "been" is to ?
  * clustering algorithms and comparing it to thesaurus
  * visualization&#x20;
* Extrinsic evaluation: test word embeddings on external task&#x20;
  * named entity recognition&#x20;
  * POS tagging&#x20;
  * ...&#x20;

## Completed Notebook

[CBOW Embedding Training ](https://drive.google.com/file/d/1rDpEZw03Yf\_ZvoA0GAaHf9XtAWuOe9RX/view?usp=share\_link)

* Manual coding of gradient descent&#x20;
* Text pre-processing&#x20;

