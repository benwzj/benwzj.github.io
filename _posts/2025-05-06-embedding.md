---
layout: post
title: Embedding 
date: 2025-05-06
categories: AI
tags: AI Embedding Vector ML
toc: 
  - name: What are Embeddings
  - name: Word Embedding
  - name: Pre-trained Models
  - name: What is Word2vec exactly
  - name: Create Your Own Embeddings
  - name: FAQ
---

Embeddings is the basic key point to understand Machine Learning. And it is Machine Learning's Most Useful Multitool.

Embeddings are a powerful technique in AI, enabling machines to understand and work with complex data in a more meaningful way. They play a crucial role in various applications, from natural language processing to computer vision and recommendation systems.

In machine learning, an embedding is a way of representing data as points in n-dimensional space so that similar data points cluster together. 

## What are Embeddings

Abstractively, embeddings allow us to find similar data points. The machine can understand things because of embedding.
Realistically, embeddings store semantically meaning in arrays of number.

What kinds of things can be embedded? All The Things! Text, Images, Videos, Music.

Embeddings are typically learned from large datasets using machine learning techniques.

### Understand Embeddings

When talk about embeddings, i though it is just lots of array of numbers which telling about the things.
But how to define this dimensions? what algorithm to operate this dimensions? 
Embedding should be a whole system, it including the start and the end. it include algorithms, data training, etc.
It can embed your input into arrays, it can understand those arrays, it can capture semantic relationships, and it can inference.

So Creating embeddings starts with a large dataset. For example word embeddings, this is a text corpus. We define a model, often a **shallow neural network** like in Word2Vec. Word2Vec's skip-gram architecture, for instance, predicts surrounding context words given a target word. The model's hidden layer weights become the word embeddings.

## Word Embedding

Word embeddings are extremely useful in natural language processing. They can be used to find synonyms (“semantic similarity”), to do clustering, or as a preprocessing step for a more complicated nlp model.

Example:
Imagine the words "cat," "dog," and "car." Their embeddings might look like this (simplified):
```
cat: [0.8, 0.2, 0.1]
dog: [0.7, 0.3, 0.2]
car: [0.1, 0.1, 0.9]
```

Notice that "cat" and "dog" have similar embeddings, reflecting their semantic similarity, while "car" has a very different embedding.

## Pre-trained Models

This pre-trained models also called Techniques which are used to create embeddings.

A pre-trained embedding model is a model that has already been trained on a large dataset and can be used to generate embeddings for new data without further training (or with minimal fine-tuning). Think of it as a ready-to-use embedding generator.

### Some famous ones are: 

- Word2Vec: a technique invented by Google in 2013. It Learns word embeddings by predicting surrounding words given a target word (or vice versa).
- GloVe (Global Vectors for Word Representation): Learns word embeddings by capturing global word co-occurrence statistics.
- FastText: An extension of Word2Vec that considers subword information, allowing it to generate embeddings for out-of-vocabulary words.
- Sentence Transformers: Generate embeddings for entire sentences or paragraphs.
- Graph Embeddings: Represent nodes in a graph as vectors.

### How to use a pre-trained embedding model:
- Choose a Model: Select a pre-trained model that suits your task and data. Popular choices include Word2Vec, GloVe, FastText, and sentence transformers like BERT. Consider factors like the size of the vocabulary, the dimensionality of the embeddings, and the type of data the model was trained on.
- Load the Model: Use a library like `Gensim` or `Hugging Face Transformers` to load the pre-trained model.
- Generate Embeddings: Input your data (e.g., words, sentences) into the loaded model to generate embeddings.
- Use the Embeddings: Use the generated embeddings as input features for your machine learning model or for other tasks like semantic search.

Let's say you want to build a sentiment analysis model. You could use pre-trained word embeddings from **GloVe**. You would load the GloVe model, then for each word in your input text, you would retrieve its corresponding pre-trained embedding vector. These vectors would then be used as input features for your sentiment analysis model.

### Example
Using `google/universal-sentence-encoder` pre-trained model: 
```python
import tensorflow_hub as hub

embed = hub.load("https://tfhub.dev/google/universal-sentence-encoder/4")
embeddings = embed([
    "The quick brown fox jumps over the lazy dog.",
    "I am a sentence for which I would like to get its embedding"])

print(embeddings)

# Response looks like: [[0.001, 0.201, ...]]
# i.e., an array of vectors
```

## What is Word2vec exactly

Word2Vec is categorize as embedding model while BERT as a Language Model. 
Word2Vec provide methods, algorithms, neural network for embedding purpose. BERT can work to embedding, also can  do much more various NLP tasks like generating contextualized word and sentence embeddings.

### Technique

You can say Word2vec is a technique. This technique is used for obtaining vector representations of words in natural language processing (NLP). These vectors capture information about the meaning of the word based on the surrounding words. 

### Model

You can say Word2vec is a group of models.  
Word2vec is composed of a group of related models that are used to produce word embeddings. 
These models are shallow, two-layer neural networks that are trained to reconstruct linguistic contexts of words.

### How Word2vec approach

Word2vec takes as its input a large corpus of text and produces a mapping of the set of words to a vector space, typically of several hundred dimensions, with each unique word in the corpus being assigned a vector in the space.
(Word2vec 将大量文本作为输入，并将词集映射到向量空间（通常有几百维），语料库中每个唯一的词都会在空间中分配一个向量。)

Here are a little bit more detail:
Word2vec can use either of two model architectures to produce these distributed representations of words: continuous bag of words (CBOW) or continuously sliding skip-gram. In both architectures, word2vec considers both individual words and a sliding context window as it iterates over the corpus.

- The CBOW can be viewed as a ‘fill in the blank’ task.
- In the continuous skip-gram architecture, the model uses the current word to predict the surrounding window of context words.
- CBOW is faster while skip-gram does a better job for infrequent words.
- A corpus is a sequence of words. Both CBOW and skip-gram are methods to learn one vector per word appearing in the corpus.

### How to use Word2Vec

Using Word2Vec involves two main steps: training the model (optional, as pre-trained models are available) and then using the trained model to generate word embeddings.

#### Training (Optional)
This is optional, as pre-trained models are available. But you can still train it with your own data.

#### Using the Model
Load the Model:
```Python
from gensim.models import Word2Vec
model = Word2Vec.load("word2vec.model") # Or load a pre-trained model
```

Get Word Embeddings:
```py
vector = model.wv['cat']  # Get embedding for 'cat'
```

Find Similar Words:
```py
similar_words = model.wv.most_similar('cat', topn=5) # Find the 5 most similar words to 'cat'
```

Perform Word Arithmetic (Analogy):
```py
result = model.wv.most_similar(positive=['king', 'woman'], negative=['man'], topn=1) # king - man + woman = ? (queen)
```

Use in Downstream Tasks:
Use the embeddings as features in machine learning models for tasks like:
- Text Classification: Sentiment analysis, spam detection.
- Machine Translation: Encoding and decoding text in different languages.
- Information Retrieval: Semantic search.
- Recommendation Systems: Recommending similar items.

## Create Your Own Embeddings

Usually, when we are talking about Creating Our Own Embeddings, usually refer to fine-tuning some pre-trained model. 

Actually, you can creat one from scratch. Or saying train one.
Here's what you'll need:
- Familiarity with Python and a deep learning framework like TensorFlow or PyTorch is **essential**.
- Data: A sufficiently large and relevant dataset is crucial.
- Algorithm: Choose an appropriate embedding algorithm. Word2Vec, GloVe, and FastText are good starting points for word embeddings.
- Computational Resources Access to a decent CPU, and ideally a GPU, will significantly speed up the process.
- Patience and Experimentation: requires patience and a willingness to experiment with different hyperparameters and architectures to achieve optimal results.


Here are the highly simplified example: 
```py
import numpy as np

# Sample vocabulary and data
vocabulary = {"the": 0, "cat": 1, "sat": 2, "on": 3, "mat": 4}
sentences = [["the", "cat", "sat", "on", "the", "mat"]]

# Hyperparameters
vocab_size = len(vocabulary)
embedding_dim = 5
learning_rate = 0.1
window_size = 2

# Initialize embeddings randomly
embeddings = np.random.rand(vocab_size, embedding_dim)

# Training loop (simplified)
for epoch in range(100):  # Example number of epochs
    for sentence in sentences:
        for i in range(len(sentence)):
            target_word = sentence[i]
            context_words = sentence[max(0, i - window_size):i] + sentence[i + 1:min(len(sentence), i + window_size + 1)]

            # Simplified training logic (replace with actual gradient updates)
            for context_word in context_words:
                target_index = vocabulary[target_word]
                context_index = vocabulary[context_word]
                # ... (Calculate gradients and update embeddings) ...

print(embeddings) # Your trained embeddings
```

But A real implementation, it would involve things like
- A neural network: For predicting context words.
- Backpropagation: For calculating gradients.
- An optimization algorithm: Like SGD for updating embeddings.

## FAQ

- When talking embedding, is it refer to an array of number?
- Who define the dimensions?
- Word2Vec is a pre-trained model or just a Techniques which difine demensions?
- How to create our own embedding model?
- What is the difference between text and image embedding? 
- What is the relationship of Word2vec and Transformer?
- Can say BERT is a embedding model?




