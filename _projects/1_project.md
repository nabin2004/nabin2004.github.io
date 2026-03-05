---
layout: page
title: ManiBench
description: A benchmark for testing visual-logic drift and syntactic hallucinations in Manim code generation across 9 LLMs.
img:
importance: 1
category: research
related_publications: true
---

**ManiBench** is a novel benchmark evaluating **9 large language models** (Claude Sonnet 4, Gemini 2.5 Pro, Kimi-K2.5, DeepSeek-R1, and others) on **Manim CE** mathematical animation code generation.

### Key Contributions

- Formalized **4 evaluation metrics**: Executability, Version-Conflict Error Rate (VCER), Alignment Score, and Coverage Score across **276 generated samples**.
- Grounded benchmark design in analysis of **53,000 lines** of 3Blue1Brown's ManimGL source code — cataloging 143 scene classes, 120 visual techniques, and 145 documented GL→CE API incompatibilities across 8 conflict categories.
- Designed a **5-strategy prompting ablation** (zero-shot, few-shot, CoT, constraint-based, version-aware) on Gemma-3-27B across 180 runs. Few-shot was the only strategy improving executability (+11.1pp); version-aware prompting eliminated version conflicts entirely (VCER = 0.0%).
- Built a **fully automated evaluation pipeline** with AST parsing, sandboxed subprocess rendering, regex-based static analysis, and keyword-bank heuristics.

Presented at **LOCUS Research Symposium, Feb 2026** (NAST, Lalitpur).

**Links:** [GitHub](https://github.com/nabin2004/ManiBench) · [Paper (TeX)](https://github.com/nabin2004/ManiBench-paper)
