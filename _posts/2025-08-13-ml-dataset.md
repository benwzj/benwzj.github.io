---
layout: post
title: Datasets Overview
date: 2025-06-16
categories: AI
tags: Python ML 
toc: 
  - name: Numerical Data 
    subsections:
      - name: Understand Feature Vector
      - name: Feature engineering technique 
      - name: Steps of handling numerical Data 
      - name: Numerical data Best practices
  - name: Categorical Data
    subsections:
      - name: Low-dimensional categorical features
      - name: High-dimensional categorical features
      - name: What is feature crosses
---

There are some concepts need to understand about dataset:
- There are four different characteristics of data and datasets.
- There are four different causes of data unreliability.
- what is label.
- You can improve the quality of human-rated labels.
- When you train a model, you will subdivide a dataset into a training set, validation set, and test set.
- What is overfitting.
- What is regularization.


## Data characteristics

Tables are an intuitive input format for machine learning models. You can imagine each row of the table as an example and each column as a potential feature or label. 

Such as 
- comma-separated values (CSV)
- directly from spreadsheets
- database tables.

### Types of data
- numerical data
- categorical data
- human language, including individual words and sentences
- multimedia (such as images, videos, and audio files)
- outputs from other ML systems
- embedding vectors

### Quantity of data

### Quality and reliability of data

### Complete vs. incomplete

Real-world examples are often incomplete, meaning that at least one feature value is missing.
What you can do?
- Delete incomplete examples or,
- Impute missing values

## Labels

