---
layout: page
title: "kapa.ai (YC S23) - Inspired RAG Platform"
description: A production-grade, multi-tenant documentation RAG system built from first principles. 12-combination retrieval experiment. RAGAS-gated CI/CD. MCP server for Claude/Cursor integration. All four RAGAS metrics passing gate thresholds.
importance: 1
category: GenAI
github: https://github.com/AyanArshad02/kapa-inspired-rag-mcp
---

## Why I Built This

After shipping the RAG chatbot at Softeon, I wanted to build the same class of system publicly — with every decision documented, every experiment logged, and the evaluation infrastructure treated as seriously as the retrieval infrastructure.

Kapa.ai does this for developer documentation at scale. I wanted to understand *exactly* how a system like that is built, from chunking strategy through to CI/CD gates on retrieval quality.

---

## The Experiment: What Actually Works

Before writing any production code, I ran a structured experiment: **12 combinations of chunking strategy × retrieval method × reranker**, evaluated against a frozen set of 78 Q&A pairs from real FastAPI and Supabase documentation.

The 78 Q&A pairs were created once and frozen before any experiments ran. No leakage, no post-hoc selection.

### Chunking strategies tested
- `RecursiveCharacterTextSplitter` (baseline)
- `HeadingAwareChunker` (custom — splits on markdown/HTML headings)
- `SlidingWindowChunker`
- `CodeBlockAwareChunker`

### Retrieval methods tested
- Dense only (cosine similarity)
- Sparse only (BM25)
- Hybrid with Reciprocal Rank Fusion (RRF)

### Reranker
- Cohere `rerank-english-v3.0` applied as final pass

### What won

`HeadingAwareChunker` + dense retrieval + Cohere reranker.

The surprising result: **hybrid retrieval didn't outperform dense + reranker** on this documentation corpus. Dense retrieval paired with a strong reranker matched hybrid quality with less complexity. Hybrid adds value when keyword precision matters (config keys, exact error codes) — for prose-heavy documentation, the reranker handled that job.

`HeadingAwareChunker` produced 50% fewer chunks than `RecursiveCharacterTextSplitter` while improving Mean Reciprocal Rank from 0.687 to 0.755. Smaller index, better results.

---

## Architecture

```
Client → FastAPI → Retrieval Pipeline → GPT-4o → Streaming SSE response
                        ↓
                   Qdrant (per-tenant collections)
                        ↓
              HeadingAwareChunker → text-embedding-3-small
                        ↓
              Cohere Reranker → top-k context
                        ↓
                Redis (conversation memory + response cache)
```

**MCP Server** — Claude Desktop and Cursor can query the knowledge base directly via the MCP protocol. Ask questions about your docs from inside your IDE without switching context.

**RAGAS-gated CI/CD** — retrieval quality is evaluated on every push against the frozen 78 Q&A eval set. A push that drops any metric below threshold fails the pipeline. This isn't a monitoring metric — it's a deployment gate.

---

## RAGAS Results

Evaluated on the frozen 78 Q&A set against FastAPI and Supabase documentation:

| Metric | Score | Gate threshold |
|--------|-------|----------------|
| Faithfulness | 0.908 | ≥ 0.85 ✅ |
| Answer Relevancy | 0.832 | ≥ 0.80 ✅ |
| Context Precision | 0.892 | ≥ 0.85 ✅ |
| Context Recall | 0.949 | ≥ 0.90 ✅ |

---

## Key Design Decisions and Tradeoffs

**Frozen eval set before any code**

Same lesson from Softeon: build the evaluation infrastructure first. The 78 Q&A pairs were created before the first retrieval experiment ran. This prevented me from unconsciously tuning toward metrics rather than real retrieval quality.

**Dense + reranker over hybrid**

Hybrid retrieval is the conventional wisdom for RAG. The experiment showed it wasn't necessary for this corpus. I'd rather have a simpler system I understand fully than a complex one that performs the same. Hybrid is still the right call when the query distribution skews toward exact keyword matches.

**Per-tenant Qdrant collections**

Same architectural decision as the Softeon chatbot — isolation at the storage layer, not the query layer. The overhead is worth the safety guarantee.

**RAGAS as a first-class contract, not a monitoring metric**

Most teams run RAGAS as an offline analysis tool. Putting it in CI means a retrieval regression blocks the deployment automatically, without human review. The tradeoff: it slows down the deploy cycle slightly. The payoff: you can't accidentally ship a quality regression.

---

## Stack

FastAPI · Qdrant · OpenAI `text-embedding-3-small` · GPT-4o · Cohere Reranker · Redis · PostgreSQL · Docker Compose · RAGAS · MCP Server · Python 3.11+
