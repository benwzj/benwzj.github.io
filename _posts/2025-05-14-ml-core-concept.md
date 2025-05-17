---
layout: post
title: Core concepts behind ML 
date: 2025-05-06
categories: AI
tags: AI ML
---


ML is the **process** of training a piece of software, called a model, to make useful predictions or generate content (like text, images, audio, or video) from data.

## Types of ML Systems

ML systems fall into one or more of the following categories based on how they learn to make predictions or generate content:
- Supervised learning
- Unsupervised learning
- Reinforcement learning
- Generative AI

### Supervised learning
Supervised learning models can make predictions after seeing lots of data with the correct answers and then discovering the connections between the elements in the data that produce the correct answers.

Supervised ML models are trained using datasets with labeled examples. 

Two of the most common use cases for supervised learning are regression and classification.

#### Regression
A regression model predicts **a numeric value**. For example, a weather model that predicts the amount of rain, in inches or millimeters, is a regression model.

#### Classification
Classification models predict the likelihood that something belongs to a category.

Classification models are divided into two groups: binary classification and multiclass classification.

### Unsupervised learning
Unsupervised learning models make predictions by being given data that does not contain any correct answers. 
An unsupervised learning model's goal is to identify meaningful patterns among the data. In other words, the model has no hints on how to categorize each piece of data, but instead it must infer its own rules.

A commonly used unsupervised learning model employs a technique called clustering. The model finds data points that demarcate natural groupings.

Clustering differs from classification because the categories aren't defined by you. 

### Reinforcement learning
Reinforcement learning models make predictions by getting rewards or penalties based on actions performed within an environment. A reinforcement learning system generates a policy that defines the best strategy for getting the most rewards.

Reinforcement learning is used to train robots to perform tasks, like walking around a room, and software programs like AlphaGo to play the game of Go.

### Generative AI
Generative AI is a class of models that creates content from user input. 

At a high-level, generative models learn patterns in data with the goal to produce new but similar data. Generative models are like the following:
- Comedians who learn to imitate others by observing people's behaviors and style of speaking
- Artists who learn to paint in a particular style by studying lots of paintings in that style
- Cover bands that learn to sound like a specific music group by listening to lots of music by that group

To produce unique and creative outputs, generative models are initially trained using an unsupervised approach, where the model learns to mimic the data it's trained on. The model is sometimes trained further using supervised or reinforcement learning on specific data related to tasks the model might be asked to perform, for example, summarize an article or edit a photo.


## Linear Regression

Training data to form a model, simply say, it is to find the bias and weights among the data.
linear regression is one of the methods that find the relationship between features and a label to get the bias and weights.

{% include figure.html path="assets/img/car-data-points-with-model.png" class="img-fluid rounded z-depth-1" width="80%" %}

During training, the model calculates the **weight** and **bias** that produce the best model.
{% include figure.html path="assets/img/linear-regression-equation.png" class="img-fluid rounded z-depth-1" width="80%" %}


### Loss

Loss is a numerical metric that describes how wrong a model's predictions are. Loss measures the distance between the model's predictions and the actual labels. 
The goal of training a model is to minimize the loss, reducing it to its lowest possible value.

{% include figure.html path="assets/img/loss-lines.png" class="img-fluid rounded z-depth-1" width="80%" %}

The two most common methods to remove the sign are the following:
- Take the absolute value of the difference between the actual value and the prediction.
- Square the difference between the actual value and the prediction.

There are four main types of loss:
- **L1 loss**: The sum of the absolute values of the difference between the predicted values and the actual values.	
- **Mean absolute error (MAE)**: The average of L1 losses across a set of examples.	
- **L2 loss**: The sum of the squared difference between the predicted values and the actual values.	
- **Mean squared error (MSE)**: The average of L2 losses across a set of examples.

#### Choosing a loss

In training, model will try to get the best bias and weights according to the LOSS. So choosing a loss is matter.

When choosing the best loss function, also consider how you want the model to treat outliers.
The outliers are closer to the model trained with MSE than to the model trained with MAE.
{% include figure.html path="assets/img/model-mse.png" class="img-fluid rounded z-depth-1" width="80%" %}
A model trained with MSE moves the model closer to the outliers.
{% include figure.html path="assets/img/model-mae.png" class="img-fluid rounded z-depth-1" width="80%" %}
A model trained with MAE is farther from the outliers.

### Gradient descent

Gradient descent is an iterative process that finds the best weights and bias that minimize the loss.

Gradient descent is a **mathematical technique** that iteratively finds the weights and bias that produce the model with the lowest loss. Gradient descent finds the best weight and bias by repeating the following process for a number of user-defined iterations.

The model begins training with randomized weights and biases near zero, and then repeats the following steps:
- Calculate the loss with the current weight and bias.
- Determine the direction to move the weights and bias that reduce loss.
- Move the weight and bias values a small amount in the direction that reduces loss.
- Return to step one and repeat the process until the model can't reduce the loss any further.

This is typical loss curve, Loss is on the y-axis and iterations are on the x-axis: 
{% include figure.html path="assets/img/loss-convergence.png" class="img-fluid rounded z-depth-1" width="80%" %}

Loss surface showing the weight and bias values that produce the lowest loss.
{% include figure.html path="assets/img/loss-surface-points.png" class="img-fluid rounded z-depth-1" width="80%" %}

### Hyperparameters

Hyperparameters are variables that control different aspects of training. 
Three common hyperparameters are:
- Learning rate
- Batch size
- Epochs

In contrast, parameters are the variables, like the weights and bias, that are part of the model itself. 
In other words, hyperparameters are values that you control; parameters are values that the model calculates during training.

#### Learning rate
The learning rate determines the magnitude of the changes to make to the weights and bias during each step of the gradient descent process. 

The model multiplies the gradient by the learning rate to determine the model's parameters (weight and bias values) for the next iteration.

Learning rate is a floating point number you set that influences how quickly the model converges.

The ideal learning rate helps the model to converge within a reasonable number of iterations.

What do it means when Learning Rate is 1?
A learning rate of 1 means that the model updates its weights by the full amount of the calculated gradient. This almost inevitably leads to highly unstable training and the model failing to converge to a good solution.

#### Batch size
Batch size is a hyperparameter that refers to the number of examples the model processes before updating its weights and bias.

You might think that the model should do Full Batch, means calculating the loss for every example in the dataset before updating the weights and bias. However, when a dataset contains hundreds of thousands or even millions of examples, using the full batch isn't practical.

Two common techniques to get the right gradient on average without needing to look at every example in the dataset before updating the weights and bias:
- Stochastic gradient descent (SGD): Stochastic gradient descent uses only a single example (a batch size of one) per iteration. The term "stochastic" indicates that the one example comprising each batch is chosen at random. (Note that using stochastic gradient descent can produce noise throughout the entire loss curve, not just near convergence.) 
- Mini-batch stochastic gradient descent (mini-batch SGD): Mini-batch stochastic gradient descent is a compromise between full-batch and SGD. The model chooses the examples included in each batch at random, averages their gradients, and then updates the weights and bias once per iteration.

#### Epochs
During training, an epoch means that the model has processed every example in the training set once.
For example, given a training set with 1,000 examples and a mini-batch size of 100 examples, it will take the model 10 iterations to complete one epoch.

Training typically requires many epochs. In general, more epochs produces a better model, but also takes more time to train.

Here is an example to tell the difference:
- Full batch: After the model looks at all the examples in the dataset. For instance, if a dataset contains 1,000 examples and the model trains for 20 epochs, the model updates the weights and bias 20 times, once per epoch.
- Stochastic gradient descent: After the model looks at a single example from the dataset. For instance, if a dataset contains 1,000 examples and trains for 20 epochs, the model updates the weights and bias 20,000 times.
- Mini-batch stochastic gradient descent: After the model looks at the examples in each batch. For instance, if a dataset contains 1,000 examples, and the batch size is 100, and the model trains for 20 epochs, the model updates the weights and bias 200 times.




