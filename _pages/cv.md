---
layout: cv
permalink: /cv/
title: cv
nav: true
nav_order: 5
cv_pdf: NabinOli_resume.pdf
description: For some reasons, this page stays outdated atleast by 6 months ;-)
toc:
  sidebar: left
---

# Nabin Oli

HuggingFace | LinkedIn | GitHub | nabinoli2004@gmail.com | +977-9848185797

Undergraduate researcher working on low-resource NLP, multilingual speech modeling, and knowledge-augmented language systems; experience fine-tuning Nepali ASR models, contributing to Hugging Face TRL, and building logically validated knowledge graphs.

## Experience

### Center for AI Research Nepal (CAIR) — Research Intern, Knowledge Representation
Aug 2025 – Present (Remote)

- Collaborated with team to design and formalize HeritageGraph, an OWL ontology for Nepal’s cultural heritage aligned with CIDOC-CRM; validated for logical consistency and achieved full competency question coverage (32/32) across structural, ritual, institutional, and Living Goddess domains under the supervision of Dr. Tek Raj Chhetri.
- Established cross-standard semantic alignment across 13 vocabularies including CIDOC-CRM (42 classes, 71 properties) and PROV-O.
- Verified OWL-RL logical consistency via deductive closure, producing 17,894 inferred triples with zero unsatisfiable classes; authored SHACL constraint schemas for provenance tracking and contributor ingestion, reducing data integrity violations by ~30%.

### Wiseyak Inc. — Machine Learning Intern, Speech Processing
Jan 2025 – July 2025 (Hybrid)

- Contributed to a real-time Nepali Automatic Speech Recognition (ASR) pipeline integrating acoustic modeling, language modeling, and post-processing for banking customer service applications, achieving <2s end-to-end latency.
- Fine-tuned the Indic Conformer model using NVIDIA NeMo for Nepali speech-to-text, reducing Word Error Rate (WER) by ~18% over baseline via domain adaptation.
- Trained a 117M-parameter GPT-2 for ASR post-processing and text correction using a corpus curated from 40,000+ Nepali Wikipedia articles, improving post-processed transcription accuracy by ~12%.
- Built KenLM n-gram rescoring models (bi/tri/4-gram), improving fluency and reducing WER by ~9% on held-out banking domain test sets.
- Developed an XLM-RoBERTa-based masked language model for English–Nepali transliteration, achieving ~87% top-1 accuracy on a held-out benchmark.

## Open Source Contribution

### Hugging Face TRL — Open Source Contributor
Feb 2026

- PR #5091: Added a Multi-Node Training subsection to TRL distributed training docs (Accelerate, HPC guidance). (Merged)
- PR #5002: Added entry for CGPO / Mixture of Judges to the TRL Paper Index and linked to `trl.experimental.judges.AllTrueJudge`. (Merged)

## Publishing

- "Knowledge Graphs: Structure, Function, and Significance" — Book Chapter 3, Under Review (Elsevier).
- "ManiBench: A Benchmark for Testing Visual-Logic Drift and Syntactic Hallucinations in Manim Code Generation" — Presented at LOCUS Research Symposium, Feb 2026 (NAST, Lalitpur).
- "Dialograph: A decision-theoretic framework for proactive educational dialogue using temporal graphs" — Paper in review at 2026 International Conference on Applied Artificial Intelligence (2AI).

## Education

### Sunway College Kathmandu (Affiliated with Birmingham City University, UK)
2023 – 2027 (Expected)

BSc (Hons.) Data Science — Consistently maintained First-Class Honours, Kathmandu, Nepal

Relevant Coursework: Data Structures & Algorithms, Linear Algebra, Probability & Statistics, Database Systems, OOP (Java), Data Visualization with R, Web Application Development.

## Awards, Certifications, and Recognitions

- Fusemachines AI Fellowship — 2025: Selected 1 of 150 fellows from 3,000 applicants.
- Data Fellowship — 2023: Full-year DataCamp scholarship.
- Kaggle Datasets Ranking — Global rank 946 / 15,470; Kaggle Dataset Expert.
- DataCrunch Hackathon 2024 — 3rd place (Code for Nepal).

## Technical Skills

- Languages: Python, R
- ML / NLP: PyTorch, HuggingFace Transformers, NVIDIA NeMo, Scikit-learn, LangChain, spaCy
- Knowledge Graphs: GraphDB, RDF, SPARQL, SHACL, CIDOC-CRM, PROV-O, LinkML
- Data: NumPy, Pandas, Matplotlib
- MLOps: MLflow, Weights & Biases, Docker, GitHub Actions

## References

- Dr. Tek Raj Chhetri — Postdoctoral Associate, McGovern Institute for Brain Research, MIT
- Supriya Khadka — Undergrad Research Coordinator at Sunway 

## Projects (selected)

- ManiBench: Visual-Logic Drift & Syntactic Hallucination Benchmark — Python, OpenRouter API. Introduced a benchmark evaluating 9 LLMs on Manim CE code generation with 4 evaluation metrics and an automated evaluation pipeline.
- GPT-2 from Scratch Implementation + Instruction Fine-Tuning — PyTorch, HuggingFace. Implemented 124M GPT-2 from scratch and conducted instruction fine-tuning for downstream tasks.
- MiniTorch: Autograd & Neural Network Engine from Scratch — Python. Built a PyTorch-style autograd and validated via numerical gradient checks.

---

If you'd like, I can also generate an updated `NabinOli_resume.pdf`, run a git commit with these changes, and push to your GitHub repository next. 
