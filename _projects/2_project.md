---
layout: page
title: Dialograph
description: A decision-theoretic framework for proactive educational dialogue using temporal graphs.
img:
importance: 2
category: research
---

**Dialograph** is a lightweight Python library for representing, evolving, and traversing dialogue memory as a **temporal graph**. Designed for **proactive dialogue agents**, where reasoning over user preferences, beliefs, and strategies matters more than raw generation.

### Core Concepts

- **Typed Nodes** — user preferences, beliefs/facts, and dialogue strategies — each carrying confidence and temporal metadata.
- **Directional, Weighted, Time-Aware Edges** — semantic relations (supports, contradicts, elicits), dialogue flow dependencies, and strategy activation paths.
- **Decision-Theoretic Reasoning** — temporal decay, contradiction detection, and centrality-based scoring for proactive dialogue planning.

Paper in review at the **2026 International Conference on Applied Artificial Intelligence (2AI)**.

**Links:** [GitHub](https://github.com/nabin2004/dialograph) · [Paper (TeX)](https://github.com/nabin2004/Dialograph-paper)
