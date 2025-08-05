---
layout: post
title: Transformer Spelled Out in One Take
date: 2025-07-31 10:00:00
description: A walkthrough of the Transformer architecture — what it is, how it works, and why it revolutionized NLP. From self-attention to positional encoding, everything laid bare in one pass.
tags: transformer, NLP, deep learning, attention, architecture
categories: nlp
typograms: true
disqus_comments: true
---

It's 2025 and ChatGPT has become a thing for which everyone is saying , "Yeah, yeah we know that." Pretty common nowadays. And If you are Tech bros then I guess Transformer architecture have also become "Yeah, I heard it". Some of you already know ["Attention is all you need paper"](https://arxiv.org/pdf/1706.03762). But in my circle most have never bothered to know what is transformer architecture and what powers it? and why it changed the pace and direction of AI atleast for now? 

In this blog, I intend to cover the transformer architecture in depth. Starting with Attention and Self-attention. 

PS: If you are never learned about Transformer then I suggest you just learn what is there and dont worry much while learning self-attention and when we start positional encoding after it will start making sense. And Before we end, I most possibly able to succesfully show you how Transformer is also an neural network and why every components of transformer is there in architecture.

# Attention:
Let's start with Attention intuitively.
Being human we know the term "attention" just means focusing on something more then other. In our human case, we just tend to give more effort if we want to give more attention. Just like you are giving more attention to this blog then your phone lying next to your laptop on your desk. But, In computers or in AI, Its all about numbers, When we say giving attention in AI/NLP/ML ,tt just means doing operations with bigger numbers, and giving less attention means doing operation with smaller numbers. But, where does this attention numbers (its generally called scores) comes from?

### Here’s the math behind it:
Each word (or token) gets transformed into three vectors:
- Query (Q): What I’m looking for
- Key (K): What I have to offer
- Value (V): What I’ll share

For every token, you compare its Query to all the Keys in the sequence to decide how much attention to pay to each token. Then you use those attention weights to mix the Values together — producing a new, context-rich representation for that token.

It's like:
```
"I’m token X. I want to know who explains me best. Let me check everyone’s Keys, figure out who matches me, then gather their Values weighted by how relevant they are."
```

This process is what we call **Scaled Dot-Product Attention**. Here's the formula:

Attention(Q, K, V) = softmax( (Q × Kᵀ) / √dₖ ) × V

Where
    - Q: Query matrix (shape: sequence_length × dₖ)
    - K: Key matrix (shape: sequence_length × dₖ)
    - V: Value matrix (shape: sequence_length × d_v)
    - dₖ: dimensionality of the Key vectors (used for scaling)

## Steps: 
1. Q × Kᵀ: For each Query, take the dot product with all Keys. This gives us attention scores. How much each token "cares" about the others.
2. Divide by √dₖ: This scaling step keeps values from becoming too large, which would make the softmax overly peaky (sensitive). This helps with stable gradients during training.
3. softmax: Turns the scores into a probability distribution values sum to 1. This is the weight each token gives to others.
4. Multiply by V: The softmax output is used to take a weighted sum of Values. This produces a new vector that represents a token, but now it’s infused with context from the other tokens.


# Self-Attention:
We just saw the basic attention idea: scoring how much one token should focus on others. Now, self-attention is just a special case of attention where the tokens attend to themselves the entire sequence pays attention within itself.

Simply:
    1. Each token’s Query is compared against Keys from the same sequence (including itself).
    2. This creates a map of how tokens relate to each other in that sequence.
    3. Then we mix the Values weighted by those attention scores.

### Why Not Just Call It "Attention"?
In practice, "attention" is a broader concept: it could be attention between two different sequences like a question and a passage in question-answering. But self-attention is attention within the same sequence, which is the building block of Transformers.

Attention mechanisms help parallelize sequence processing by allowing the model to consider all tokens simultaneously, rather than step-by-step like RNNs. In RNNs, each token depends on the previous hidden state, forcing strict sequential computation that can’t be parallelized easily. Attention sidesteps this by computing pairwise interactions between all tokens at once using matrix multiplications, which are highly parallelizable on GPUs. This eliminates the bottleneck of waiting for previous steps to complete, speeding up both training and inference. Technically, this also solves the long-range dependency problem in RNNs, where information from distant tokens tends to vanish or explode during backpropagation. Attention directly connects every token to every other token, maintaining stable gradient flow and making it easier for the model to learn relationships across long sequences.

# Positional Encoding: Adding Order to Attention
Attention alone treats input tokens as a bag of words. It knows which tokens are important to each other, but not their order. That’s a problem because language isn’t just about which words appear; it’s about where they appear. 

Unlike RNNs, which process tokens sequentially and inherently know order, Transformers handle all tokens simultaneously, so they need an explicit way to inject position information. This is where positional encoding comes in.

Positional encoding adds a unique numerical pattern to each token’s embedding, representing its position in the sequence. These encodings get added to the original token embeddings before the attention mechanism kicks in, letting the model differentiate between "cat sat on mat" and "mat sat on cat." The original Transformer paper uses sine and cosine functions of different frequencies to generate these positional vectors. This design lets the model generalize to longer sequences than it’s seen before, and it encodes relative positions smoothly.

In short, positional encoding solves the “order problem” by giving the model a sense of where each token lives in the sequence — enabling it to understand language in a structured, meaningful way.

# Add maths here

# Nonlinearities:
One surprising thing about the core attention mechanism is that it doesn’t use traditional nonlinear activation functions like ReLU or sigmoid. Instead, it’s essentially a series of weighted averages. When you compute attention, you’re taking a weighted sum of the Value vectors, where weights come from the softmax scores. Softmax itself is nonlinear, but beyond that, the attention operation is a linear combination of vectors.

Why does this matter? 
Deep learning usually relies on nonlinearities to let networks approximate complex functions. But in Transformers, nonlinearities mostly happen outside attention like in the feed-forward layers that follow each attention block. Attention focuses on dynamically mixing information across tokens, using these weighted averages to emphasize relevant parts of the input.

Think of it like a smart mixer: it doesn’t transform ingredients with a magic spell, but it knows exactly how much of each ingredient to blend in to get the right flavor. The magic comes from learning which tokens to pay attention to, not from twisting those token embeddings with nonlinearities.

This linearity in attention is part of why Transformers are efficient and stable to train the heavy lifting of complexity happens in the network’s other layers, while attention focuses on smart, context-aware blending.

# Masking the future in self-attention: 
In tasks like language modeling or text generation, the model shouldn’t peek into the futur that is, it must not use information from words that come after the current word it’s predicting. This is where masking comes into play. In self-attention, each token looks at all other tokens to decide what to pay attention to. But for autoregressive tasks, allowing a token to attend to future tokens would leak information, breaking the model’s integrity. To fix this, Transformers apply a causal mask, a simple trick that blocks attention scores from looking ahead. Practically, this means when computing attention for token i, the model only considers tokens from position 1 up to i, never beyond.

Technically, the mask is applied by adding a large negative number (like −∞) to the attention scores corresponding to future tokens before the softmax step. Since softmax of very large negative numbers is near zero, those future tokens get effectively zero attention weight.

Masking preserves the autoregressive property: the model can only use past and current information to predict the next token. Without it, you’d be cheating byusing answers before seeing the questions. This subtle yet crucial step ensures that self-attention works correctly for generation tasks and keeps the training and inference processes honest.

<div style="text-align: center;">
  <img src="./transformers/Attention based models.jpg" alt="Attention based models picture">
</div>

Above is the core idea behind most attention-based models in NLP.
Now, let’s move into the Transformer architecture as introduced in the "Attention Is All You Need" paper.

<div style="text-align: center;">
  <img src="./transformers/image.png" alt="transformer architecture from Attention is all you need paper">
</div>

Above diagram might seem somewhat familiar at this point aside from a few components like the encoder-decoder setup, Add & Norm layers, Multi-Head Attention, and a few others, which we’ll dive into in the upcoming sections.

# Transformer Decoder:
The Transformer decoder is the foundation for building systems like language models. At its core, it closely resembles the minimal self-attention architecture we’ve already explored, but with a few important additions. The token embeddings and positional encodings remain the same, preserving the way input sequences are initially represented. However, the next step is to enhance our self-attention mechanism by introducing multi-head self-attention, which allows the model to capture diverse relationships in different subspaces of the data simultaneously.

# Multi-head Self Attention:
So far, we’ve talked about self-attention—a mechanism where each token looks at every other token to gather relevant information. That’s powerful, but here’s the catch: a single attention head might only learn one type of relationship. For example, it might focus only on subject–verb links or only on nearby context. What if we want to capture multiple types of relationships at once?

That’s where Multi-Head Self-Attention (MHSA) comes in.

Instead of computing one set of Q, K, and V and attending over them, we compute multiple sets in parallel. Each “head” has its own weight matrices and learns to attend differently. One might focus on syntax. Another on long-term dependencies. Another on punctuation (seriously).

### Here's the process:
Linearly project the input embeddings into multiple sets of Q, K, V using separate learned weight matrices.

1. Each head performs scaled dot-product attention independently.
2. The outputs of all heads are concatenated.
3. This concatenated vector is passed through another linear layer to combine the information.

# "Add Maths here"

### Why is MHSA useful?
- Diversity of focus: Multiple heads = multiple perspectives on the same data.
- Better representation: Different attention heads specialize, which leads to richer contextual embeddings.
- Parallelizable: All heads operate in parallel so no slowdown.

# Transformer Decoder: Add & Norm
Now that we've swapped in multi-head self-attention, we move closer to the full Transformer decoder. But before stacking layers, we need to talk about two subtle yet essential tricks that make training deep Transformers stable and effective:
1. Residual Connections
2. Layer Normalization

In most Transformer diagrams, you’ll see these two written compactly as "Add & Norm". 

## Residual Connections: Let’s Not Forget the Original Input
Deep models suffer from vanishing gradients, especially when stacked many layers deep. Residual connections solve this by adding the original input of a layer back to its output. So, the model doesn’t have to relearn everything from scratch at each layer. It can learn just the differences.

In formula terms, if x is the input and F(x) is the output of some sub-layer (like attention or a feedforward network), the residual connection gives:

```
Output = x + F(x)
```

## Layer Normalization: Keep the Numbers in Check
After adding the residual, we apply layer normalization. This keeps activations well-scaled and consistent across the network, which helps gradients flow and stabilizes training.
So the full step becomes:
```
Output = LayerNorm(x + F(x))
```

"Add & Norm" might sound like a minor cleanup step, but it’s crucial for making the Transformer stackable. Without it, training deep layers becomes unstable and gradients get lost.


The Transformer Decoder is built by stacking multiple Decoder Blocks, each with the same internal structure. Every block includes a self-attention layer, followed by an Add & Norm, then a feed-forward network, and finally another Add & Norm. These components repeat across layers, allowing the model to progressively refine its understanding of the input.

# Encoder
While the Transformer Decoder is designed to work with unidirectional context looking only at past tokens, as required in language modeling. The Transformer Encoder lifts this constraint. It allows each token to attend to both past and future tokens, enabling bidirectional context, much like a bidirectional RNN. The only architectural difference is that the Encoder drops the masking step in self-attention, making it fully non-causal. This change unlocks richer representations, especially useful for understanding tasks like classification or question answering.


# Cross-attention
In self-attention, the queries, keys, and values all come from the same source. The same sequence of token embeddings. But in the **Transformer decoder**, there's a second attention mechanism called **cross-attention**, which works a bit differently. Here, the decoder is attending to the encoder’s output effectively letting the decoder "look back" at the input sequence it’s trying to respond to.

Let’s break that down. Suppose the encoder outputs are $h_1, h_2, \ldots, h_n$, each a vector in $\mathbb{R}^d$. These are treated as the memory from which the decoder reads. The decoder’s current inputs are $z_1, z_2, \ldots, z_n$, also vectors in $\mathbb{R}^d$. In **cross-attention**, we form the **keys** and **values** from the encoder outputs: $k_i = K h_i$, $v_i = V h_i$, while the **queries** come from the decoder: $q_i = Q z_i$. So the decoder is asking: *"Given what I’m currently generating (z), how relevant are the encoded inputs (h)?"*

In matrix form, let $H \in \mathbb{R}^{T \times d}$ be the encoder output (stacked $h_i$'s), and $Z \in \mathbb{R}^{T \times d}$ be the current decoder input. The attention is computed as:

$$
\text{output} = \text{softmax}(ZQ \cdot (HK)^T) \cdot HV
$$

Here, $ZQ$ are the queries from the decoder, and $HK$, $HV$ are the keys and values from the encoder. This gives us a matrix of **attention scores across all encoder-decoder token pairs**, followed by a weighted average over the values. That’s how the decoder decides what parts of the encoded input to pay attention to while generating each output token.

Cross-attention is a key part of the encoder-decoder communication pipeline. It lets the decoder ground its generation in the actual input, instead of flying blind.
