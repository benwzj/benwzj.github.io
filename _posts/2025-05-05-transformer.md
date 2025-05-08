---
layout: post
title: What is Transformer 
date: 2025-05-05
categories: AI
tags: AI Transformer
---


## What is Transformer 

Transformer is a type of neural network architecture.

- Transformers were initially designed for translation, superseded RNN.
- Unlike traditional recurrent or convolutional models that process data sequentially, the Transformer leverages a mechanism called **self-attention** to process all input data simultaneously. This allows for much greater parallelization, leading to faster training and the ability to handle longer sequences of data effectively.
- Transformers are widely used in various fields, including natural language processing (NLP) for tasks like translation, text generation, and question answering, as well as computer vision. 
- In essence, transformer models have revolutionized the way we process and understand sequential data by leveraging the power of attention and parallel processing. 
- Like GPT(Generative Pre-trained Transformer), BERT(Bidirectional Encoder Representations from Transformers), they are based on Transformers.
- Self-Attention and Positional Encoding are the main innovations.

## Key Concepts

- Self-Attention: The core innovation of Transformers. It allows the model to weigh the importance of different parts of the input when generating an output. For example, in a sentence, the word "it" might refer to different things depending on the context. Self-attention helps the model understand these relationships. It does this by calculating relationships between every word in a sequence and every other word, creating a weighted representation of the input.
- Attention Mechanism: A more general concept that allows the model to focus on specific parts of the input when generating an output. Self-attention is a specific type of attention.
- Encoder-Decoder Architecture: Many Transformers follow this structure. The encoder processes the input sequence and generates a contextualized representation. The decoder then uses this representation to generate the output sequence.
- Parallelization: Unlike recurrent networks that process input sequentially, Transformers can process all input tokens simultaneously, significantly speeding up training.
- Positional Encoding: Because Transformers don't process sequentially, positional information of words in a sentence is lost. Positional encodings are added to the input embeddings to provide information about the position of each word.
- Feedforward Networks: Fully connected layers within each encoder and decoder layer that further process the information from the attention mechanism.
- Layer Normalization: A normalization technique used to stabilize training and improve performance.

## How a Transformer works (simplified)

- Input Embedding: The input sequence (e.g., a sentence) is converted into numerical representations called embeddings.
- Positional Encoding: Positional information is added to the embeddings.
- Encoder: Multiple encoder layers process the embeddings using self-attention and feedforward networks. Each encoder layer produces a set of encoded representations.
- Decoder: The decoder takes the encoded representations from the encoder and, using self-attention and feedforward networks, generates the output sequence (e.g., a translation, a summary, or the next word in a sentence). The decoder also uses attention mechanisms to focus on relevant parts of the encoded input.
- Output: The final decoder layer produces the output.

## Why are Transformers important

Improved Performance: They have achieved state-of-the-art results in various NLP tasks.
Parallelization: They train much faster than recurrent models.
Handling Long Sequences: They can effectively process long sequences of data.

## RNN
A recurrent neural network (RNN) is a type of neural network architecture specifically designed to process **sequential** data. 
it have many problems. Like:
- it struggle to learn long-range dependencies.
- Because sequential, it can't be parallelized training. it is very slow.

RNNs have been largely superseded by Transformer networks.

