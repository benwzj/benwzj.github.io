---
layout: post
title: Logistic Regression 
date: 2025-05-22
categories: AI
tags: AI ML
toc: 
  - name: What is Logistic Regression
  - name: Sigmoid function
  - name: Logistic regression trainning
---

## What is Logistic Regression

Logistic regression is designed to predict the probability of a given outcome.

Classic example is spam-prediction model.

Simply saying, Logistic regression = Logistic function + linear regression.

## Sigmoid function

There's a family of functions called logistic functions whose output represents a probability, always outputting a value between 0 and 1. 
The standard logistic function, also known as the sigmoid function (sigmoid means "s-shaped"), has the formula: 

$$f(x) = \frac{1}{1 + e^{-x}}$$

- The 'e' is Euler’s number, a fundamental mathematical constant. `e≈2.71828...`
- Logistic functions is one of The most important exponential function.

Here are the classic corresponding graph of the sigmoid function:
{% include figure.html path="assets/img/sigmoid_function_with_axes.png" class="img-fluid rounded z-depth-1" width="80%" %}
- As the input, x, increases, the output of the sigmoid function approaches but never reaches 1. 
- Similarly, as the input decreases, the sigmoid function's output approaches but never reaches 0.
- The sigmoid function will bend the linear equation straight line into an s-shape.

### Transforming linear output using the sigmoid function

Left: graph of the linear function z = 2x + 5, with three points highlighted. Right: Sigmoid curve with the same three points highlighted after being transformed by the sigmoid function:
{% include figure.html path="assets/img/linear_to_logistic.png" class="img-fluid rounded z-depth-1" width="80%" %}


## Logistic regression trainning

Logistic regression models are trained using the same process as linear regression models, with two key distinctions:
- Logistic regression models use Log Loss as the loss function instead of squared loss.
- Applying regularization is critical to prevent overfitting.

### Log Loss
In the Linear regression module, you used squared loss (also called L2 loss) as the loss function. 
loss function for logistic regression is Log Loss. 

{% include figure.html path="assets/img/logloss-func.png" class="img-fluid rounded z-depth-1" width="80%" %}

### Regularization

Regularization, a mechanism for penalizing model complexity during training.

Regularization is extremely important in logistic regression modeling. 
Without regularization, the asymptotic nature of logistic regression would keep driving loss towards 0 in cases where the model has a large number of features. 

Consequently, most logistic regression models use one of the following two strategies to decrease model complexity:
- L2 regularization
- Early stopping: Limiting the number of training steps to halt training while loss is still decreasing.

