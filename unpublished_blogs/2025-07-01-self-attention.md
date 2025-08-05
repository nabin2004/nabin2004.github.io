---
layout: post
title: Self Attention Mechanism
date: 2024-04-29 23:36:10
description: Self-Attention Mechanism
tags: transformers, deep learning, attention, NLP
categories: transformer
typograms: true
disqus_comments: true
---

The world took notice in November 2022 when OpenAI released ChatGPT. Based on the Transformer architecture, it continues to power countless applications today. This architecture originates from the paper *"Attention Is All You Need"*, and though it has been modified in many ways since, the core idea—attention—remains central.

## Why Did Transformers Become So Successful?

Before answering that, a quick note: this is the first post in a Transformer series. I plan to cover components like multi-head attention, positional encodings, architecture breakdowns, and possibly pretraining, RLHF, and fine-tuning. All that, if I stay consistent. Because, yes—**Consistent is All I Need**.

Before Transformers, RNNs reigned supreme. You can get a feel for their dominance from the tongue-in-cheek paper *"RNNs Were All We Needed"*, or Karpathy’s classic blog *"The Unreasonable Effectiveness of Recurrent Neural Networks"*.

### But RNNs had problems:

#### Bottlenecks

- RNNs process one token at a time—each step depends on the previous one.
- This kills parallelism. GPUs can’t help much here.
- Training is slow, inference is worse.

#### Vanishing/Exploding Gradients

- Gradients can shrink to nothing or explode on long sequences.
- LSTMs and GRUs helped, but didn’t solve it completely.
- If a token appeared 40 steps ago, the RNN might forget it like last Tuesday’s lunch.

#### Scaling Limitations

- Stack many RNN layers, try long documents, and watch training time explode and quality suffer.

---

## What We Wanted

- See the entire sequence at once  
- Learn which tokens matter (regardless of position)  
- Parallelize and scale efficiently  
- Handle long-term dependencies without forgetting the start

---

## Enter: Self-Attention

What if each token could look at every other token and decide how important each one is?

Self-attention allows a model to **weigh the relevance of all tokens** when encoding any specific one.

Let’s walk through a metaphor:

> Meet Tok. Tok is a token.  
> Last week, Tok lived in an RNN. It waited patiently, only to be forgotten by the time the model got to it.  
> Today, Tok lives the dream. A world where every token sees every other token—instantly.  
> No waiting. No forgetting. Tok gets attention not for being first or last, but for being **relevant**.  
> You, the reader, understood “he” referred to Tok. How? You too attend globally, just like self-attention.

Self-attention gives every token a **global view**. It computes attention scores between all token pairs, deciding who matters to whom. The result? A new, context-aware representation for every token.

---

## If You Understand Python Dictionaries, You’re Halfway There

```python
names = {'first': 'nabin', 'second': 'oli'}
````

In self-attention, we’re doing something similar: querying over keys to retrieve values. Except now, it’s all vectorized.

### The Core Elements:

* **Q** (Query): What I'm looking for
* **K** (Key): What others are offering
* **V** (Value): What I’ll take if the offer matches

Each token has its own Q, K, and V. Attention is matching Qs with Ks to decide which Vs to blend in.

---

## Scaled Dot-Product Attention

This is where Q, K, and V interact.

1. Take the dot product between a token's Query and every other Key.
2. This gives a score of how well each other token "matches" the query.
3. Do this for every token pair — you get an n × n matrix of scores.
4. To stabilize large values (from high-dimensional vectors), divide by √dₖ (dimension of Key).
5. Apply softmax to turn scores into probabilities.
6. Use these weights to compute a weighted sum of the Value vectors.

### Why scale?

Without scaling, large dot products can push softmax into extreme values—leading to unstable training.

**Summary:**
Dot product = similarity.
Scaling = numerical stability.
Softmax = who actually matters.
Weighted sum = updated token representation.

---

## A Worked Example

Given:

* "The" = \[0.1, 0.2]
* "cat" = \[0.3, 0.4]
* "sat" = \[0.5, 0.6]

Suppose attention weights for “cat” are:

* 10% to “The”
* 40% to itself
* 50% to “sat”

Then its updated vector is:

```text
= 0.10 * [0.1, 0.2] +
  0.40 * [0.3, 0.4] +
  0.50 * [0.5, 0.6]

= [0.38, 0.48]
```

The “cat” now embeds information from the whole sentence.

---

## Why Does It Work?

* **Global context:** Every token sees every other token.
* **Handles long-range dependencies:** “He” can refer back to “Tok”, no matter how far away.
* **GPU-friendly and parallelizable:** Unlike RNNs, attention works all at once.

---

## Frequently Asked Questions

**Q: Where do Q, K, V come from?**
A: Each token is embedded into a vector. From that, we use learned weight matrices to produce Q, K, and V:

```text
Q = embedding × Wq  
K = embedding × Wk  
V = embedding × Wv
```

**Q: Why separate Q, K, and V?**
A: Each has a different job—asking, identifying, and providing. Using one vector for all three is like asking one person to be the detective, witness, and evidence.

**Q: How does the model learn Wq, Wk, Wv?**
A: Like any neural net—via backpropagation during training.

**Q: If attention looks at everything, how does it know the order of words?**
A: Transformers add **positional encodings**, covered in a future post.

**Q: Can attention handle very long sequences?**
A: Technically yes, but vanilla attention scales with O(n²). Beyond a few thousand tokens, it becomes expensive. There are efficient variants.

**Q: How is this better than RNNs or CNNs?**
A: RNNs are slow and sequential. CNNs are limited to local context. Attention is global, parallel, and distance-agnostic.

**Q: What happens if we don’t scale the dot product?**
A: Softmax becomes unstable with large values, leading to bad gradients and poor training.

**Q: Is this too heavy for small devices?**
A: Vanilla self-attention is computationally heavy. Lightweight or sparse variants exist for edge devices.

**Q: What about other domains like images or audio?**
A: Transformers work there too. Vision Transformers treat patches as tokens; audio models treat frames as tokens.

---

## TL;DR

* Ask the right questions (**Q**)
* Match them with the right sources (**K**)
* Listen to the right information (**V**)
* Decide who to trust (softmax)
* Then act (weighted sum of values)

---

Stay tuned for the next post, where we'll dive into **Multi-Head Attention** and **Positional Encoding**.