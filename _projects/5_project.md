---
layout: page
title: Dagestan
description: Temporal graph memory layer for LLMs — time-aware knowledge graphs replacing flat vector embeddings.
img:
importance: 5
category: work
---

**Dagestan** stores LLM memory as a **typed temporal knowledge graph** instead of flat vector embeddings. It tracks entities, concepts, events, preferences, and goals — with time-aware confidence decay, contradiction detection, and relationship-based retrieval.

### Why Dagestan?

Current LLM memory solutions (vector DBs) lack:
- **Time awareness** — old and new information treated identically
- **Relationships** — no structure between memories
- **Contradiction detection** — conflicting facts coexist silently
- **Decay** — nothing fades; nothing is curated

### Features (v0.1)

- Typed temporal graph (Entity, Concept, Event, Preference, Goal nodes)
- LLM-based knowledge extraction from conversations
- Contradiction detection between conflicting preferences/goals
- Exponential temporal decay on confidence scores
- Gap detection and bridge node detection (cross-cluster connections)
- Centrality-based importance scoring
- Query-driven graph retrieval (keyword + structure, no embeddings)

**Links:** [GitHub](https://github.com/nabin2004/dagestan)
