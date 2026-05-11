---
layout: page
title: Multi-Tenant RAG Chatbot
description: Designed and shipped a production RAG chatbot serving 500+ enterprise WMS clients. Faithfulness from 0.67 to 0.91. Unsupported claim rate below 4%.
importance: 2
category: GenAI
proprietary: true
---

## The Problem

Softeon's WMS platform serves 500+ enterprise clients — each with their own configuration, workflows, and documentation. Clients were raising support tickets for questions already answered in their own documentation. The support load was high, and the answers were sitting in docs nobody could search effectively.

The ask: a chatbot that answers WMS questions accurately, in natural language, across all clients simultaneously.

The constraint that shaped everything: **data isolation**. Enterprise clients could not have their data bleed into each other's answers under any circumstances — contractual, legal, and trust reasons.

---

## What Made This Hard

Most RAG tutorials show you a single-tenant setup. Multi-tenancy breaks almost every default assumption:

- **Metadata filtering** (the obvious approach) was ruled out early. At scale, a misconfigured filter or an edge case in the query router means one client's data shows up in another's response. The failure mode is silent and catastrophic.
- **Faithfulness** was our core quality metric, and the baseline was poor — 0.67 on initial RAGAS evaluation. The chatbot was generating plausible-sounding answers that weren't grounded in the retrieved context.
- **Retrieval quality** varied wildly across document types. Structured WMS config docs, free-text user guides, and FAQ pages all needed different treatment.

---

## Key Decisions

**Tenant isolation at the vector DB layer, not the query layer**

Each tenant gets a dedicated Qdrant collection. The isolation guarantee is architectural — a query for Tenant A physically cannot reach Tenant B's collection. No filter logic, no routing logic, no edge cases. The tradeoff is higher storage overhead and more collection management complexity. We accepted that.

**Hybrid retrieval + cross-encoder reranking**

Dense-only retrieval missed exact WMS terminology (module names, config keys, workflow codes). BM25 alone missed semantic intent. We ran both, merged results with Reciprocal Rank Fusion, then passed the top candidates through a cross-encoder reranker for final scoring. More latency than dense-only, but precision improved significantly on domain-specific queries.

**Faithfulness as a deployment gate**

We tracked RAGAS faithfulness in CI. Below threshold = no deploy. This forced us to diagnose retrieval failures systematically rather than ship and hope. Most faithfulness failures traced back to chunk boundaries cutting off context mid-sentence — fixed by switching to a semantics-aware chunking strategy.

---

## Results

| Metric | Before | After |
|--------|--------|-------|
| RAGAS Faithfulness | 0.67 | 0.91 |
| Unsupported claim rate | ~18% | < 4% |
| Tenants served | — | 500+ enterprise |

---

## What I'd Do Differently

The cross-encoder reranking added latency that occasionally caused timeouts under load. In hindsight, I'd introduce async reranking with a fallback to dense-only results rather than making reranking synchronous on the critical path. It would have made the P99 latency more predictable.

---

*Built at Softeon. Source code is proprietary.*
