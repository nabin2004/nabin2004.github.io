---
layout: page
title: ChessMania
description: End-to-end Chess MLOps pipeline — from PGN ingestion to Transformer-powered next-move prediction.
img:
importance: 7
category: work
---

**ChessMania** is an end-to-end MLOps project that demonstrates every stage of the machine learning lifecycle using chess game data. It starts with a **tabular ML baseline (XGBoost)** and scales into **Transformer-based sequence modelling**.

### Architecture

| Component | Implementation |
|---|---|
| **Storage** | MinIO (PGN / JSON / Parquet) |
| **Orchestration** | Apache Airflow (PGN parsing → Feature & Sequence extraction) |
| **Data Validation** | Great Expectations |
| **ML Framework** | XGBoost → GPT-style Transformer with LoRA/QLoRA |
| **Experiment Tracking** | MLflow (Accuracy, F1, AUC, Perplexity, MFU) |
| **Serving** | FastAPI + Uvicorn (Next-move prediction, Win probability) |
| **Monitoring** | Evidently AI (Feature drift + Structural/Move-sequence drift) |

**Links:** [GitHub](https://github.com/nabin2004/ChessMania) · [Chess-DataOps](https://github.com/nabin2004/Chess-DataOps) · [Chess-ontology](https://github.com/nabin2004/Chess-ontology)
