---
description: https://www.coursera.org/learn/classification-vector-spaces-in-nlp/home/week/2
---

# NLP-C1-W2: Sentiment Analysis with Navïe Bayes

## Learning Theme&#x20;

* Feature extraction from a document&#x20;
* Confidence ellipse for visualizing Naive Bayes&#x20;

## Negative and Positive Frequency Representation (W1 material)&#x20;

**Features:**  For each word, identify its positive and negative frequency by counting how many times it appears in positive and negative cases. The final feature for is a vector of length three: a bias unit (normally 1), the sum of deduped words’ positive frequency, and the sum of deduped words’ negative frequency

**Algorithm:** Logistic regression on the features to classify as positive or negative sentiment.

## Naive Bayes Frequency Ratio Representation&#x20;

**Features:** For each word, derive its positive and negative frequency

$$
\begin{align*}
& p(w_i | pos) = \frac{freq(w_i,pos)}{freq(pos)}\\
& freq(w_i,pos): \text{Total count of word i in all positive text} \\
& freq(pos): \text{Total count of all words in all positive text} \\ 
\end{align*}
$$

**Algorithms:** Naive Bayes&#x20;

$$
\begin{align*}
    & \mathbb{I} \left(\frac{p(pos)}{p(neg)}\prod_{i=1}^n \frac{p(w_i|pos)}{p(w_i|neg)} \geq 1 \right) \\
    & \frac{p(pos)}{p(neg)}: \text{The ratio between postiive and negative document.}
\end{align*}
$$

**Laplacian Smoothing**: The feature above suffers the problem of multiplying/dividing by 0 if a certain word only shows up in one class. Laplacian smoothing replaces the the conditional probability for word i with the following definition.&#x20;

$$
\begin{align*}
& p(w_i | pos) = \frac{freq(w_i,pos)+1}{N_{pos} + V}\\
& N_{pos}: \text{Sum of word frequency in positive class}\\
& V: \text{Number of unique word in the entire text (both classes)} \\ 
\end{align*}
$$

**Log sum Trick**: Direct likelihood is generally a very small number, and multiplying it contains the risk of underflow. So, we use the log of the number to transform multiplication into addition for safer calculations.

$$
\begin{align*}
\log\left( \frac{p(pos)}{p(neg)}\prod_{i=1}^n \frac{p(w_i|pos)}{p(w_i|neg)} \right) = \log \frac{p(pos)}{p(neg)} + \left( \sum_{i=1}^n \log \frac{p(w_i|pos)}{p(w_i|neg)} \right)
\end{align*}
$$

Notice the threshold for the indicator function becomes 0 instead of 1.

## Confidence Ellipse&#x20;

A confidence ellipse is a 2-D generalization of a confidence interval. It draws an ellipsoid around points to capture a certain percentage of them. It can be drawn with [matplotlib tutorial ](https://matplotlib.org/stable/gallery/statistics/confidence\_ellipse.html)or R packages.&#x20;

## Completed Notebook&#x20;

[Logistic Regression Sentiment Analysis Notebook](https://drive.google.com/file/d/1VjBPjnc84eIcTmzQi-KuPAQk1LbJKl93/view?usp=sharing)

* Stemming, Removing Stopwords
* Manual Implementation of Logistic Regression and loss-derivation

[Naive Bayes Sentiment Analysis Notebook](https://drive.google.com/file/d/10B7u3PMDpUxS3i6jWMwQK5OBupr800Ar/view?usp=sharing)

* Naive Bayes Sentiment Analysis

&#x20;&#x20;
