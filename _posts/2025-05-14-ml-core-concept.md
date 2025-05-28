---
layout: post
title: Core concepts behind ML 
date: 2025-05-14
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

## ML Model types
Linear Regression
Logistic Regression 
Classification