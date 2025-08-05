---
layout: post
title: The Geometry of words in NLP
date: 2025-07-28 12:19:00
description: A beginner-friendly guide to understanding how word embeddings like Word2Vec and GloVe capture meaning, and why simple vector math like 'king - man + woman = queen' actually works in practice.
tags: word, representation, word embeddings, NLP
categories: nlp
typograms: true
disqus_comments: true
---

You may have seen this phrase thrown around in NLP circles:

"King - Man + Woman = Queen"

At first glance, it looks like some bizarre algebraic fan fiction. But this odd-looking equation is one of the most famous examples of what word embeddings can do and why they're powerful.

In this post, we’ll unpack why this equation works, what’s really going on under the hood, and why it’s more than just a party trick.

## Words as vectors
Before word embeddings, computers represented words using one-hot vectors. A simple, high-dimensional vectors where each word has a unique position marked with a 1, and all other entries are 0. 
```
man  → [0, 0, 0, 1, 0, 0, 0, ...]
woman → [0, 0, 0, 0, 1, 0, 0, ...]
```
And Why Not Just Use Words instead of numbers?
Humans understand "apple" as a concept. Computers see "apple" as a string of characters: ['a', 'p', 'p', 'l', 'e']. That’s meaningless to a model.

Machine learning models especially neural networks requires numerical input to perform operations like:
1. Matrix multiplications
2. Dot products
3. Gradients and Updates
Strings can't be added, multiplied, or optimized over. But, Numbers can.

the problem with this representation is:
1. sparse (Mostly zeros: Each word is represented by a huge vector with just one 1 and the rest all zeros. That’s a lot of wasted space.)
2. High-dimensional (Too big: If your vocabulary has 100,000 words, every word gets a vector of length 100,000. It gets big and messy fast.)
3. Totally useless for capturing meaning or similarity (The computer knows that "apple" and "banana" are different words but not that they’re both fruits. All words are equally unrelated in this setup.)

To fix the problems with one-hot vectors, researchers came up with a smarter idea: distributed representations, or word embeddings. Instead of huge, mostly-zero vectors, embeddings represent each word as a small, dense vector like a list of 100 or 300 numbers.

What’s special about this new space?
1. Similar Words are Close together:
Words like "king" and "queen" or "cat" and "dog" end up near each other in the space.
2. We can do sort of a cool math with them:
We can actually add and subtract word vectors and get meaningful results like:

```
 king - man + woman ≈ queen
```

which meant model isn't just storing words, it's starting to understand how they relate to each other.  
But before delving into those, I will like to include co-occurence matrix. 

## Co-occurence Matrix
Before fancy deep learning models took over, word meaning was often derived from a much simpler idea: 
"You shall know a word by the company it keeps."

A co-occurence matrix is a big table that counts how often words appear near each other in a corpus. Each row represents a word, and each column represents a context word. The value at each cell tells us how often those two words co-occur within a given window (let's say, 2–5 words).

|     | the | cat | sat | on  | mat |
| --- | --- | --- | --- | --- | --- |
| the | 0   | 12  | 7   | 9   | 4   |
| cat | 12  | 0   | 10  | 3   | 6   |
| sat | 7   | 10  | 0   | 8   | 5   |
| ... | ... | ... | ... | ... | ... |

### How to build a Co-occurence matrix (Pseudocode):
1. List All Words: First, collect all the unique words in corpus(text). This is called vocabulary (set of unique words in a corpus).
2. Make a table:  Create a big table (matrix) where both the rows and columns are the words in your vocabulary. Start with all zeroes.
3. Count Word Pairs: Go through each document (or sentence). For every word, look at the other words around it (within the same document). For each word pair, add 1 to the corresponding cell in the table.
For example:  If you see the words "cat" and "dog" in the same document, you'd increase the count at row "cat", column "dog".
4. [optional] Normalize the rows by the sum:  After you're done counting, you can scale the rows so they add up to 1. This makes the values easier to compare.

## Advantages:
- Captures word relationships using raw counts
- Based on actual corpus statistics, not learned representations
- Easy to build and understand no neural networks required

## Disadvantages:
- Sparsity: Most words never appear with most others → lots of zeros
- High-dimensionality: For a vocab of size V, it’s a V × V matrix
- No generalization: Similar words may have very different rows unless they co-occur with the exact same words

To mitigate some of these problems with the co-occurrence matrix, here comes SVD (Singular Value Decomposition).

## Singular Value Decomposition (SVD):
SVD is a technique from linear algebra that helps reduce the dimensionality of large, sparse matrices exactly what we need in the case of word co-occurrence. Instead of working with a massive V×V matrix (where V is the vocabulary size), SVD allows us to project this matrix into a much lower-dimensional space, while still capturing the most important patterns in the data.

### So What Does SVD does?
It factorizes the co-occurrence matrix MM into three components:
        M=UΣVT

    UU: captures the relationships between words
    ΣΣ: diagonal matrix of singular values (sort of like "importance weights")
    VTVT: captures the relationships between context words

By keeping only the top k singular values, we can compress the matrix into something more manageable:
    Mreduced​=Uk​Σk​
This results in dense word vectors that retain much of the original structure and often, even latent semantic relationships.

#### What does SVD bring?
1. Reduces dimensionality (from tens of thousands to just ~100–300)
2. Eliminates noise and redundancy
3. Improves similarity comparisons between words

#### But It’s Not Perfect
1. Computationally expensive on large corpora
2. Static one vector per word, no matter the context
3. Can’t capture polysemy (e.g., "apple" as fruit vs. company)

Despite its limitations, SVD was an important stepping stone toward modern embeddings and it laid the groundwork for approaches like GloVe, which took the co-occurrence idea further with smarter, learnable models.


## Word2Vec
In 2013, Mikolov et al. introduced Word2Vec, a simple and efficient model that learns word embeddings by trying to predict a word's context. It has two key architectures:

1. CBOW (Continous Bag of Words): Predicts a word given its context

    CBOW works like a fill-in-the-blank game. It looks at the words surrounding a blank and tries to guess what fits in the middle. CBOW sees the surrounding words: ["The", "cat", "sat", "on", "the"] and tries to predict the missing word: "mat".

    Over time, it learns which types of words tend to appear in similar contexts and assigns them similar vectors.

    - Input: context words (before and after)
    - Target: the center word

    It’s fast and efficient, especially good for learning high-quality embeddings for common words.

2. Skip-Gram: Predict context given a word
    Skip-Gram flips the CBOW idea. Instead of guessing the center word, it starts with a word and tries to predict the words around it.

    Example:

    - Input: "sat"
    - Targets: "The", "cat", "on", "the", "mat"

    So it's constantly trying to figure out what typically appears near the input word.

    - Input: center word
    - Target: surrounding context words

    Skip-Gram tends to perform better for rare words, because it generates more training pairs per word and captures more detailed structure.

During training, words that appear in similar contexts end up with similar vectors. But something fascinating emerged: semantic relationships became linear.

## What Does "King - Man + Woman = Queen" Really Mean?
Let’s translate this into vector space:
```
vec("king") - vec("man") ≈ vec("royalty")
vec("royalty") + vec("woman") ≈ vec("queen")
```
So the model has implicitly learned that the difference between "king" and "man" represents the concept of male royalty, and when we add "woman," we get female royalty "queen".

No one told the model what royalty is. It picked it up just by looking at patterns in text.

### How does this work?
1. Distributional Semantics
Words that appear in similar contexts tend to have similar meanings. This idea, rooted in linguistics, is the backbone of how embeddings work.

2. High-Dimensional Geometry
Relationships between words get encoded in the geometry of the embedding space. Directions in this space often capture concepts:
 - Gender: man → woman, king → queen
 - Verb tense: walk → walked, run → ran
 - Country-capital: France → Paris, Germany → Berlin

3. Training Objective Aligns Structure
Word2Vec optimizes word vectors such that dot products match how often words co-occur in context windows. This creates smooth, structured vector spaces.

## Word2vec Skip gram variant:
Till now, we know skip gram idea is given a center word, it predicts its context. In skip gram word2vec, we train a shallow and tiny neural network to do this. This tiny neural network contains:

1. Input layer: one-hot vector of a word (size: vocab size).
2. Hidden layer: size d (e.g. 100-300), the embedding space.
3. Output layer: again vocab size which predicts probabilities over all words.

But the trick/catch here is, we don't care about the output layer. Once the model is trained, you throw it away and keep the embedding matrix, that's our vectors.

<!-- ### Training objective:
For each training pair (wcenter,wcontext)(wcenter​,wcontext​), you want the dot product of their vectors to be high (they co-occur). For randomly chosen unrelated word pairs, you want their dot product to be low. The training objective becomes a binary classification problem:

    fmla here

where,
- σ is the sigmoid.
- The first term rewards correct context words.
- The second penalizes randomly sampled (noise) words. -->

### Training objective:

For each training pair $(w_{center}, w_{context})$, you want the dot product of their vectors to be **high** (because they co-occur). For randomly chosen unrelated word pairs $(w_{center}, w_{noise})$, you want their dot product to be **low**.

Formally, the objective is:

$$
\log \sigma(\mathbf{v}_{context}^\top \mathbf{v}_{center}) + \sum_{i=1}^k \mathbb{E}_{w_i \sim P_n(w)} \left[ \log \sigma(-\mathbf{v}_{w_i}^\top \mathbf{v}_{center}) \right]
$$

where:

* $\sigma(x) = \frac{1}{1 + e^{-x}}$ is the sigmoid function.
* The first term rewards the model for assigning a high dot product to actual context words.
* The second term penalizes the model if randomly sampled noise words have high dot products with the center word.
* $k$ is the number of negative samples drawn from noise distribution $P_n(w)$.


Here’s a cleaned-up, concise version with no fluff and clearer formatting:

### How is Word2Vec trained?
1. Choose a sliding window of size $c$ (e.g., 5).
2. For each center word $w_t$, collect all word pairs $(w_t, w_{t+j})$ where $-c \leq j \leq c$ and $j \neq 0$.
3. Feed each $(w_t, w_{t+j})$ pair into the model as a positive example.
4. For each positive pair, sample $k$ negative examples (usually 5–20) from a noise distribution.
5. Optimize the objective using SGD or Adam.

### After training, what do we get?
* An embedding matrix $E \in \mathbb{R}^{|V| \times d}$, where $|V|$ is the vocabulary size and $d$ is the embedding dimension.
* Each row corresponds to a word vector.
* These vectors capture semantic and syntactic relationships between words.

## Limitations
1. One vector per word: can't handle polysemy ("bank" in river vs finance).
2. Doesn't model subwords or morphology.
3. Static embeddings: not context-dependent.
4. Replaced in many cases by models like BERT or FastText (which handles subwords).

But still, Word2Vec is still fast, simple, surprisingly strong and its excellent for teaching representation learning. 

Coming back to co-occurence matrix. Here we will look at GloVe.
## What is GloVe?
GloVe (Global Vectors for Word Representation) is an unsupervised learning algorithm that learns word embeddings by leveraging global word co-occurrence statistics from a corpus. GloVe (Global Vectors for Word Representation) takes a different path. Instead of predicting context words like Word2Vec, GloVe starts with a co-occurrence matrix and factorizes it using a weighted least squares objective.

But the end result is similar: words that appear in similar contexts get similar vectors, and linear relationships emerge in the embedding space.

GloVe also nails the "king - man + woman = queen" analogy in fact, it was one of their headline examples in the paper.

Both "ice" and "steam" may co-occur with generic words like "water" or "temperature," so using raw co-occurrence counts isn't enough.

GloVe's insight: meaning is captured better by the ratio of co-occurrence probabilities:

If $k = \text{"solid"}$, this ratio is high for $i = \text{"ice"}$ and low for $j = \text{"steam"}$; conversely, if $k = \text{"gas"}$, the ratio is high for $j = \text{"steam"}$ and low for $i = \text{"ice"}$.

### Do All Word Embeddings Capture This?
No not always, It works well with:
1. Word2Vec (with good hyperparams and large corpora)
2. GloVe
3. FastText (which also captures subword information)

But more recent models like BERT or GPT generate contextual embeddings, meaning:
1. "Bank" in "river bank" and "bank loan" will have different vectors
2. These aren’t static, so analogies don’t work in the same 

"King - man + woman = queen" isn’t just a cute trick. It’s a demo of something deeper:
1. Language has structure
2. Embeddings can capture that structure
3. Simple vector math can reflect real-world semantics

Word embeddings let us mathematize meaning and that opens the door to powerful models in translation, search, question answering, and more. And if nothing else, it's pretty cool that subtracting "man" from "king" gives us "queen".

Today, word embeddings in the traditional sense (i.e., static vectors like Word2Vec, GloVe, FastText) are no longer state-of-the-art in most NLP applications. These approaches assign a single vector to each word, regardless of its context, which makes them fundamentally limited especially when dealing with polysemy (e.g., "bat" as an animal vs. a sports tool) or syntactic nuance.

Modern NLP models have shifted toward contextual embeddings where a word’s vector is dynamically computed based on its surrounding words. Transformer-based models like BERT, RoBERTa, GPT, and T5 have made it possible to encode not just the meaning of words, but also how that meaning changes across sentences. These embeddings are richer, more flexible, and far more effective across a wide range of tasks, from question answering to machine translation and semantic search.

As a result, traditional word embeddings are now mainly used for educational purposes, quick prototyping, or in low-resource environments. In production systems and cutting-edge research, contextual embeddings have become the default standard.


Let's dive into contextual embeddings:

## Contextual Embeddings
Traditional word embeddings like Word2Vec, GloVe, and FastText assign a single static vector to each word, regardless of context. This makes them blind to polysemy and syntactic nuance.

Contextual embeddings solve this by dynamically generating a word’s vector based on the sentence it appears in. That is, the embedding for a word like "bank" will differ in "He sat by the river bank." versus "She deposited money in the bank."

This shift from static to dynamic representation has been a foundational transformation in modern NLP.

## How Contextual Embeddings Work?
Contextual embeddings are produced by deep language models that read entire sentences and compute a vector for each token, informed by its surrounding tokens.

- The input sentence is tokenized and passed through a Transformer model.
- Each token attends to all other tokens (via self-attention), allowing the model to build context-aware representations.
- The embedding for a word is drawn from the hidden state of one or more layers in the network.

These embeddings are not just sensitive to semantics but also to grammar, syntax, and word order, giving them far more expressive power than static vectors.

## Key Models That Produce Contextual Embeddings

| Model       | Architecture                  | Key Characteristics                                                                       |
| ----------- | ----------------------------- | ----------------------------------------------------------------------------------------- |
| **ELMo**    | BiLSTM                        | First to produce word vectors based on entire sentence context (bidirectional LSTMs)      |
| **BERT**    | Transformer (Encoder-only)    | Trained using Masked Language Modeling (MLM); captures bidirectional context              |
| **RoBERTa** | Transformer                   | Robustly optimized version of BERT; trained longer, with more data                        |
| **GPT**     | Transformer (Decoder-only)    | Unidirectional; trained to predict next token, used in generative tasks                   |
| **T5**      | Transformer (Encoder-Decoder) | Treats all tasks as text-to-text; produces embeddings for both input and output sequences |

All of these models generate token-level embeddings that adapt based on the full input, and most allow sentence-level embeddings as well via pooling or special tokens like [CLS].

## How Are Contextual Embeddings Used?
Contextual embeddings power nearly every modern NLP pipeline. Here’s how they’re applied:

- Sentence Embeddings: For tasks like semantic search, clustering, and entailment detection.
- Token Embeddings: For named entity recognition (NER), part-of-speech tagging, and parsing.
- Feature Extraction: Use pretrained embeddings as input features for custom models.
- Transfer Learning: Fine-tune contextual models on downstream tasks (e.g., classification, QA).
- Few-Shot / Zero-Shot Tasks: Leverage pre-trained contextual understanding to generalize with minimal labeled data.

Extraction is typically done using libraries like HuggingFace Transformers, often by grabbing the hidden states from the final or intermediate layers.

<!-- ## Static vs. Contextual: A Side-by-Side Comparison -->

## When (and Why) to Still Use Static Embeddings
Despite being surpassed in performance, static embeddings still have legitimate use cases:
- Low-resource environments: Mobile apps, embedded systems, or any place where memory or compute is constrained.
- Quick prototyping: Easy to set up and run without GPU or large dependencies.
- Very small datasets: Where fine-tuning a large model is impractical or leads to overfitting.
- Educational tools: Still the best way to learn about distributional semantics and vector space representations.
- Initialization: Sometimes used to initialize weights in lightweight models.

In short: static embeddings are still useful when you're resource-limited or time-limited, or just need a rough semantic approximation.

## Mathematical View of Contextual Embeddings

1. **Input Representation (Token Embeddings)**
   Before training, each input token (word or subword) is mapped to an embedding vector:

$$
E = \{ e_1, e_2, \ldots, e_n \}, \quad e_i \in \mathbb{R}^d
$$

Each embedding $e_i$ is the sum of:

* Token embedding
* Position embedding (to capture word order)
* Segment embedding (in BERT-style models)

$$
e_i = e_i^{\text{token}} + e_i^{\text{position}} + e_i^{\text{segment}}
$$

---

2. **Transformer Layer (Self-Attention Mechanism)**
   The core of contextual embeddings is the self-attention mechanism, computing each token's representation as a weighted sum of all tokens:

$$
\text{Attention}(Q, K, V) = \text{softmax}\left( \frac{Q K^\top}{\sqrt{d_k}} \right) V
$$

Where:

* $Q = E W_Q$, $K = E W_K$, $V = E W_V$
* $W_Q, W_K, W_V \in \mathbb{R}^{d \times d_k}$ are learned projection matrices
* $d_k$ is the dimensionality of the key/query vectors (often $d_k = d / h$, where $h$ is the number of heads)

Each token attends to all others based on similarity in the embedding space. This process is repeated across multiple heads and layers, progressively building richer contextual representations.

---

3. **Training Objectives**
   Different models use different objectives to train contextual embeddings. Examples:

### A. BERT: Masked Language Modeling (MLM)

BERT predicts randomly masked tokens in the input sentence. For example:
Input: “The \[MASK] chased the mouse.”
Target: “cat”

Mathematically, for each masked token $w_i$, BERT maximizes:

$$
\log p(w_i \mid w_1, \ldots, w_{i-1}, [\text{MASK}], w_{i+1}, \ldots, w_n)
$$

Using a softmax over the vocabulary $V$:

$$
p(w_i \mid \text{context}) = \frac{\exp(h_i^\top \mathbf{w}_{w_i})}{\sum_{w \in V} \exp(h_i^\top \mathbf{w}_w)}
$$

Where:

* $h_i$ is the hidden state from the final Transformer layer at position $i$
* $\mathbf{w}_w$ is the output embedding vector for word $w$

---

### B. GPT: Causal Language Modeling (CLM)

GPT predicts the next token in a left-to-right fashion:

$$
\log p(w_1, w_2, \ldots, w_n) = \sum_{i=1}^n \log p(w_i \mid w_1, \ldots, w_{i-1})
$$

Training minimizes the negative log-likelihood:

$$
\mathcal{L}_{\text{GPT}} = - \sum_{i=1}^n \log p(w_i \mid w_{<i})
$$

Attention is masked causally to prevent attending to future tokens.

---

### C. T5: Sequence-to-Sequence Objective

T5 frames all tasks as text-to-text. For input $x = (x_1, \ldots, x_n)$ and output $y = (y_1, \ldots, y_m)$, it optimizes:

$$
\log p(y_1, y_2, \ldots, y_m \mid x_1, x_2, \ldots, x_n)
$$

Using teacher forcing and decoder attention over encoder outputs.

---

4. **Contextual Embeddings = Hidden States**
   The final contextual embeddings are the hidden states at a chosen Transformer layer $L$:

$$
\text{Embedding of token } w_i = h_i^{(L)} \in \mathbb{R}^d
$$

Optionally, you can:

* Mean-pool all token embeddings to get a sentence embedding
* Use the \[CLS] token embedding (BERT)
* Combine layers, e.g., average of the last 4 layers

## Conclusion: The Embedding Landscape Today

Word embeddings have come a long way. The field has moved from static representations like Word2Vec and GloVe to deep contextual embeddings generated by transformer-based models.

These newer models don't just store meaning they compute it on the fly. They adapt to syntax, semantics, and context in real time, making them vastly more powerful for real-world NLP tasks.

Today, contextual embeddings are the default standard in production systems, research, and cutting-edge applications. Static embeddings still have a place but mostly in teaching, bootstrapping, or lightweight setups.

### Futher Suggested Readings:
1. [Original Word2vec paper]("https://arxiv.org/pdf/1301.3781")
2. [GLoVe](https://nlp.stanford.edu/pubs/glove.pdf)
