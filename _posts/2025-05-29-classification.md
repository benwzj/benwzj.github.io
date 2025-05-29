---
layout: post
title: Classification 
date: 2025-05-22
categories: AI
tags: AI ML
toc: 
  - name:
---

Classification is the task of predicting which of a set of classes (categories) an example belongs to. 
Classification is converting a logistic regression model that predicts a probability into a binary classification model that predicts one of two classes. 

There are some term you need to be understood and clear:
- Threshold
- Accuracy
- Recall, or true positive rate
- False positive rate
- Precision

## Classification threshold

Choosing a threshold is very important for Classification. 
You can use tool like Confusion matrix to understand more about threshold in your model.

### Confusion matrix

|                       | Actual positive                                                                                             | Actual negative                                                                                                   |
|-----------------------|-------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
|   Predicted positive   |   True positive (TP):   A spam email correctly classified as a spam email. These are the spam messages automatically sent to the spam folder. |   False positive (FP):   A not-spam email misclassified as spam. These are the legitimate emails that wind up in the spam folder. |
|   Predicted negative   |   False negative (FN):   A spam email misclassified as not-spam. These are spam emails that aren't caught by the spam filter and make their way into the inbox. |   True negative (TN):   A not-spam email correctly classified as not-spam. These are the legitimate emails that are sent directly to the inbox. |


For example it can look like this:
{% include figure.html path="assets/img/confusion-matrix.png" class="img-fluid rounded z-depth-1" width="50%" %}

When the classification threshold increases, both true and false positives decrease. Becuase the model will likely predict fewer positives overall. 

## Accuracy
Accuracy is the proportion of all classifications that were correct, whether positive or negative. It is mathematically defined as:

$$\text{Accuracy} = \frac{\text{correct classifications}}{\text{total classifications}} = \frac{TP + TN}{TP + TN + FP + FN}$$

## Recall, or true positive rate
The true positive rate (TPR), or the proportion of all actual positives that were classified correctly as positives, is also known as recall.

Recall is mathematically defined as:

$$\text{Recall (or TPR)} = \frac{\text{correctly classified actual positives}}{\text{all actual positives}} = \frac{TP}{TP + FN}$$

## False positive rate
The false positive rate (FPR) is the proportion of all actual negatives that were classified incorrectly as positives, also known as the probability of false alarm. It is mathematically defined as:

$$FPR = \frac{\text{incorrectly classified actual negatives}}{\text{all actual negatives}} = \frac{FP}{FP + TN}$$

A perfect model would have zero false positives and therefore a FPR of 0.0, which is to say, a 0% false alarm rate.

## Precision
Precision is the proportion of all the model's positive classifications that are actually positive. It is mathematically defined as:

$$\text{Precision} = \frac{\text{correctly classified actual positives}}{\text{everything classified as positive}} = \frac{TP}{TP + FP}$$

Precision improves as false positives decrease, while recall improves when false negatives decrease. 


