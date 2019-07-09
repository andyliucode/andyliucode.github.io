---
layout: single
title:  "Training Word Embeddings With word2vec"
date:   2019-07-09 00:30:00 -0400
categories: deep-learning
---
Most people familiar with deep learning in NLP have heard of word embeddings. They may have heard of "word2vec", one of the most popular algorithms for training word embeddings, developed by Mikolov et al at Google Brain. The typical workflow is to train word embeddings over some large corpus in an unsupervised fashion, then pass the word embeddings into a neural network, which is trained in a supervised setting. 

This talk is for people who have seen and/or used this workflow, but think of word2vec only as a black-box algorithm, whose input is a large, unlabelled, textual dataset, and whose output is a file to be used as an input for training neural networks. Hopefully, after this talk, you will understand how word2vec works, what motivated the development of word2vec, and why word embeddings improve the performance of neural networks so much.

What are word embeddings?

Some readers may be familiar with the idea that deep learning is essentially just a form of representation learning. Any neural network can be thought of as a stack of hidden layers on top of an output layer. In classification tasks, the output layer can be recognized as an ordinary logistic regression classifier. Then the hidden layers are a sophisticated form of automatic feature extraction, whose parameters are learned from the training data. Theoretically, the strength of neural networks lies in their ability to learn very complex features, which would otherwise be difficult or impossible for humans to devise and encode. 

In natural language processing (NLP), it is very common to represent words with one-hot encoding. The first layer of our neural network (which we will call the input layer) is an DxV matrix, where V is the size of our vocabulary and D is the dimensionality of our first hidden layer. Then we can imagine the input layer of a neural network as a lookup table, which maps each word to a column vector of size N. This column vector is a learned representation of a word; it EMBEDS a word into some D-dimensional vector space. This embedding is optimized over our dataset for whatever classification task we have at hand. 

Unfortunately, the quality of our embeddings is constrained by the size of our dataset. In many scenarios, our dataset is quite small; more often than not, the size of our dataset is dwarfed by the size of our vocabulary. 

One idea is to use a proxy task to train embeddings. Ideally, the proxy task is unsupervised, so training data would be cheap to obtain in large quantities. Also, optimizing for this task should yield embeddings that capture the general semantics of our vocabulary, so that the embeddings can be used for a variety of NLP tasks. 

What should our proxy task look like? We turn to a probabilistic interpretation of language for inspiration. One of the oldest ideas in NLP is to model a sentence as a sequential process, where we want to predict the conditional probability of word (i+1) given words (1, 2, ..., i). Then this yields an extremely tractable objective function:
