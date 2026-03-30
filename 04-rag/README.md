# Module 04 — RAG

> 🚧 **Coming Soon**

**Prerequisites:** [Module 03 — Memory](../03-memory/README.md) — you need the memory architecture in place before adding retrieval on top of it.

---

## What This Module Covers

We add a retrieval-augmented generation layer so your agent can search a persistent knowledge base rather than relying solely on what fits in the context window. You'll set up pgvector in Docker, build an embedding pipeline for your existing docs and memory files, implement query patterns, and wire RAG results into the agent's context assembly. We cover the shared-rag architecture used in production to serve multiple agents from one vector store.

**Topics:**
- pgvector setup with Docker Compose
- Embedding pipeline: chunking strategy, model selection, incremental updates
- Query patterns: semantic search, hybrid search, relevance thresholds
- Shared-RAG: one vector store, multiple agents, access isolation
