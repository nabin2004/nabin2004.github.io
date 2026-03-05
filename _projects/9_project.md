---
layout: page
title: PaperTrail
description: AI-powered research paper discovery, semantic search, and RAG-based Q&A over arXiv papers.
img:
importance: 9
category: work
---

**PaperTrail** automatically fetches papers from arXiv, builds a semantic search index, and lets you ask research questions to receive **LLM-synthesised, citation-backed answers**.

### Features

| Capability | Description |
|---|---|
| **Ingest** | Fetch arXiv papers by category, download PDFs, extract + clean text, create embeddings |
| **Semantic Search** | Find relevant paper chunks with cosine similarity (FAISS) + reranking |
| **Q&A / Ask** | RAG-based question answering grounded in your paper corpus |
| **Research Report** | Full pipeline: plan → retrieve → synthesise → critique → evaluate |
| **Trends** | Surface trending keywords and categories across indexed papers |
| **Offline-first** | Embeddings run locally (no API key needed for ingest + search) |

**Links:** [GitHub](https://github.com/nabin2004/PaperTrail)
