---
layout: page
title: PyTurk (MiniTorch)
description: Minimal PyTorch-style autograd engine and neural network framework from scratch for education.
img:
importance: 6
category: work
---

**PyTurk** is a minimal educational deep-learning framework inspired by micrograd and PyTorch.

### Features

- Simple scalar autograd (`Value`) with dynamic computation graphs and reverse-mode backpropagation through arbitrary DAG-structured forward passes.
- PyTorch-like `nn` modules: `Module`, `Linear`, `Sequential`, `MLP`.
- Optimizers: SGD, Adam, RMSProp with LR schedulers.
- Lightweight datasets and `DataLoader` for experiments.
- Validated via numerical gradient checking and exploding gradient diagnostics, confirming correctness against finite-difference approximations.

**Links:** [GitHub](https://github.com/nabin2004/pytork)
