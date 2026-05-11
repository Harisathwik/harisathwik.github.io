---
layout: page
title: AI Ticket Resolution System
description: Built an AI system for Softeon's WMS support division that surfaces similar past tickets and relevant documentation at the moment a new ticket is raised. Took requirements directly from the support team, shipped end to end.
importance: 3
category: GenAI
proprietary: true
---

## The Problem

Softeon's WMS support team was resolving the same classes of tickets repeatedly. A new ticket would come in, an engineer would spend 20–40 minutes investigating — and then find that the same issue had been resolved 6 months ago, documented in a closed ticket nobody could find.

The existing system was keyword search over a flat ticket database. It didn't understand synonyms, paraphrasing, or WMS-specific terminology used differently across clients.

The ask wasn't "build an AI chatbot." It was: **when a new ticket arrives, show the engineer what's already been solved.**

---

## How I Approached It

I spent the first week with the support team before writing any code. I sat in on ticket triage, read through closed ticket histories, and asked engineers to show me cases where they *wished* they'd found something faster.

That surfaced two things the spec hadn't mentioned:

1. **Ticket titles are useless.** Engineers write titles under pressure. The diagnostic value is in the notes and resolution fields — which are unstructured, verbose, and full of WMS jargon.
2. **Documentation and tickets need to be searched together.** The resolution to many tickets was a link to a specific doc section. Separating retrieval by source would miss half the useful results.

These two findings changed the architecture significantly.

---

## Key Decisions

**Unified index over tickets + documentation**

Rather than separate retrieval pipelines for tickets and docs, I indexed both into a single Qdrant collection with source metadata. A query surfaces whatever is most relevant — past ticket, doc section, or both — ranked together. The tradeoff: you need careful chunking and metadata tagging to distinguish source types in the result display.

**Embedding on resolution content, not titles**

Ticket titles get indexed as secondary signal. The primary embedding is over the resolution notes and conversation thread. This is counter-intuitive but significantly improved retrieval precision on the support team's real queries.

**Evaluation-first development**

Before building retrieval, I built the evaluation set. I sat with two senior engineers and collected 40 "golden" query-result pairs — queries they'd actually had, with the results they'd consider correct. Every retrieval experiment ran against this set first. This caught regressions early and stopped me from optimising for the wrong thing.

**Human feedback loop**

Senior engineers flagged bad retrievals weekly via a simple thumbs-down interface. These went into a retraining queue. Not sophisticated — but it meant the system improved continuously after launch rather than degrading silently.

---

## What Shipped

- Semantic search over 3+ years of historical tickets and WMS documentation
- Results surface at ticket creation time, before the engineer starts investigating
- Source attribution on every result (ticket ID or doc section link)
- Feedback interface for continuous quality improvement

---

## What I Learned

Building evaluation infrastructure before retrieval infrastructure saved the project. The first retrieval approach I tried looked reasonable in manual testing but fell apart on the golden set — I would have shipped it without the eval framework. The 3 days spent building the eval set paid back in every subsequent experiment.

---

*Built at Softeon. Source code is proprietary.*
