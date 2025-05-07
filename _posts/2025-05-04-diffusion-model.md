---
layout: post
title: Diffusion Model Concepts 
date: 2025-05-04
categories: AI
tags: AI Diffusion
---

## what is Diffusion Model

A diffusion model is a type of generative model, meaning it's used to create new data instances that resemble the data it was trained on. 

A diffusion model is a type of deep neural network what learn to add noise to a picture and then learn how to reverse that process to recontruct a clear image.

The logic like this: the model have been trained lots of images, and store them as noises and embedding. when it is ask to generate a specific image, it can retrive noises and reverse back to image.

## Some Concepts

### Forward Diffusion (or Diffusion Process): 
This process gradually adds **Gaussian noise** to an image over many small timesteps. Eventually, the image becomes pure noise, losing all its original information. This is a Markov chain process, meaning each step only depends on the previous step.

### Reverse Diffusion (or Denoising Process): 
This is the heart of the diffusion model. It learns to reverse the diffusion process, effectively removing noise step-by-step to reconstruct the original image (or generate a new image based on a prompt). The model learns the conditional probability distribution of the image at each timestep, given the noisy image at the next timestep.

### Markov Chain: 
A sequence of events where the probability of each event depends only on the state attained in the previous event. Diffusion models use this concept for both the forward and reverse processes.

### Gaussian Noise: 
A type of random noise that follows a normal distribution (bell curve).


## How it works:

### Training: 
The model is trained on a large dataset of images. During training, it learns how noise is added at each timestep in the forward diffusion process. Crucially, it also learns how to reverse this process, effectively denoising.

### Inference (Generating Images):
Start with pure noise.
Iteratively apply the reverse diffusion process, guided by the text prompt (often encoded by an LLM). At each timestep, the model predicts the slightly less noisy image based on the current noisy image and the text embedding.
Repeat this process until a clean image is generated.

## Examples of Diffusion Models

Stable Diffusion
DALL-E 2
Imagen
