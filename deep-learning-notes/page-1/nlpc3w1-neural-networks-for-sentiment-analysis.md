---
description: https://www.coursera.org/learn/sequence-models-in-nlp/home/week/1
---

# NLPC3W1: Neural Networks for Sentiment Analysis

## Structure

### Input Representation&#x20;

A vector of numbers. Each number represents a specific word. Zero-padded to make sure all inputs have the same size.&#x20;

## Model Architecture in Trax

```python
// Some code
from trax import layers as tl

Model = tl.Serial(
        tl.Dense(4), 
        tl.Sigmoid(), 
        tl.Dense(4), 
        tl.Sigmoid(), 
        tl.Dense(3), 
        tl.Softmax())
```

## Completed Notebook

[Trax: Lecture Notebook](https://drive.google.com/file/d/1EMgxSwrJEVPKPMfONm8Y1bfEGT8reN5-/view?usp=share\_link)

* Trax introduction&#x20;

