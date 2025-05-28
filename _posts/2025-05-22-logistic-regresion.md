---
layout: post
title: Logistic Regression 
date: 2025-05-22
categories: AI
tags: AI ML
toc: 
  - name: What is Logistic Regression
---

## What is Logistic Regression

Logistic regression is designed to predict the probability of a given outcome.

Classic example is spam-prediction model.

## Sigmoid function

There's a family of functions called logistic functions whose output represents a probability, always outputting a value between 0 and 1. The standard logistic function, also known as the sigmoid function (sigmoid means "s-shaped"), has the formula: $$f(x) = \frac{1}{1 + e^{-x}}$$
The 'e' is Euler’s number, a fundamental mathematical constant. `e≈2.71828...`
Logistic functions is one of The most important exponential function.

{% include figure.html path="assets/img/sigmoid_function_with_axes.png" class="img-fluid rounded z-depth-1" width="80%" %}

As the input, x, increases, the output of the sigmoid function approaches but never reaches 1. Similarly, as the input decreases, the sigmoid function's output approaches but never reaches 0.

The sigmoid function will bend the linear equation straight line into an s-shape.

### What is Exponent

| Expression   | Meaning                         | Result | Explanation                             |
| ----------   | ------------------------------- | ------ | --------------------------------------- |
| $$2^3$$      | $$2 \times 2 \times 2$$         | 8      | Multiply 2 three times                  |
| $$5^2$$      | $$5 \times 5$$                  | 25     | Square of 5                             |
| $$10^0$$     | —                               | 1      | Any non-zero number to the 0 power is 1 |
| $$2^{-3}$$   | $$\frac{1}{2^3} = \frac{1}{8}$$ | 0.125  | Negative = reciprocal                   |
| $$4^{-1}$$   | $$\frac{1}{4}$$                 | 0.25   | Negative exponent = 1 over base         |
| $$9^{1/2}$$  | $$\sqrt{9}$$                    | 3      | Fractional = root                       |
| $$27^{1/3}$$ | $$\sqrt[3]{27}$$                | 3      | Cube root                               |
| $$16^{3/4}$$ | $$\left(\sqrt[4]{16}\right)^3$$ | 8      | Root first, then power                  |

## Logistic regression trainning

Logistic regression models are trained using the same process as linear regression models, with two key distinctions:
- Logistic regression models use Log Loss as the loss function instead of squared loss.
- Applying regularization is critical to prevent overfitting.

### Log Loss
In the Linear regression module, you used squared loss (also called L2 loss) as the loss function. 
loss function for logistic regression is Log Loss. 

{% include figure.html path="assets/img/logloss-func.png" class="img-fluid rounded z-depth-1" width="80%" %}

### Regularization in logistic regression
Regularization, a mechanism for penalizing model complexity during training, is extremely important in logistic regression modeling. Without regularization, the asymptotic nature of logistic regression would keep driving loss towards 0 in cases where the model has a large number of features. Consequently, most logistic regression models use one of the following two strategies to decrease model complexity:
- L2 regularization
- Early stopping: Limiting the number of training steps to halt training while loss is still decreasing.

