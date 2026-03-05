---
layout: page
title: WheelSonnet (GPT-2 from Scratch)
description: 124M & 355M GPT-2 from scratch in PyTorch — sentiment, paraphrase detection, sonnet generation, and ONNX export.
img:
importance: 4
category: work
---

**WheelSonnet** is a from-scratch implementation of the GPT-2 architecture (124M & 355M parameters) in PyTorch, fine-tuned for **sentiment classification** (SST-5 & CFIMDB), **paraphrase detection** (Quora), and **Shakespearean sonnet generation**, with additional **instruction-tuning** and **ONNX export**.

### Implementation Details

- Multi-head causal self-attention, positional embeddings, Pre-LN transformer layers, residual streams, and a custom AdamW optimizer with decoupled weight decay.
- Reproduces core results from *"Language Models are Unsupervised Multitask Learners"* (Radford et al., 2019).
- Gradient flow tests, optimizer correctness assertions, and numerical stability sanity checks throughout training.
- Models published on HuggingFace: `nabin2004/WheelSonnet2-355M`, `nabin2004/WheelSonnet2-355M-it`, `nabin2004/WheenSonnet-355M-onnx`.

**Links:** [GitHub](https://github.com/nabin2004/CS224_final_project_GPT2) · [HuggingFace Models](https://huggingface.co/nabin2004/WheelSonnet2-355M)
