---
description: >-
  Lee, C.-H., Yang, H.-C., Su, X.-Q., & Tang, Y.-X. (2022). A Multimodal
  Affective Sensing Model for Constructing a Personality-Based Financial Advisor
  System. Applied Sciences, 12(19), 10066
---

# A Multimodal Affective Sensing Model for Constructing a Personality-Based Financial Advisor System

## Abstract

To achieve successful investments, in addition to financial expertise and knowledge of market information, a further critical factor is an individual’s personality. Decisive people tend to be able to quickly judge when to invest, while calm people can analyze the current situation more carefully and make appropriate decisions. Therefore, in this study, we developed a multimodal personality-recognition system to understand investors’ personality traits. The system analyzes the personality traits of investors when they share their investment experiences and plans, allowing them to understand their own personality traits before investing. To perform system functions, we collected digital human behavior data through video-recording devices and extracted human behavior features using video, speech, and text data. We then used data fusion to fuse human behavior features from heterogeneous data to address the problem of learning only one-sided information from a single modality. Through several experiments, we demonstrated that multimodal (i.e., three different signal inputs) personality trait analysis is more accurate than unimodal models. We also used statistical methods and questionnaires to evaluate the correlation between the investor’s personality traits and risk tolerance. It was found that investors with higher openness, extraversion, and lower neuroticism personality traits took higher risks, which is similar to research findings in the field of behavioral finance. Experimental results show that, in a case study, our multimodal personality prediction system exhibits high performance with highly accurate prediction scores in various metrics.

## Summary

Multimodal sensing for personality prediction. Personality is then suppose to have impact on investment profiles.&#x20;

## Data&#x20;

32 customers (22 male, 10 female)

10 min of video data later edited into 3452 video clips with images, speech, and transcribed texts

## Features

Facial Expression Features

* CNN to extract spatial features and then input to LSTM to extract temporal feature

Text Content Features&#x20;

* BiLSTM

Speech Data Features&#x20;

* MFCC, ZCR, and spectral centroid then to CNN&#x20;
