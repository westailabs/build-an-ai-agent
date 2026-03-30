# Module 05 — Tools

> 🚧 **Coming Soon**

**Prerequisites:** [Module 04 — RAG](../04-rag/README.md) — foundational agent infrastructure should be solid before adding a broad tool layer.

---

## What This Module Covers

We build out the tools layer: the exec policy that controls what the agent can run, the skills system that packages tools into reusable units, and the approval flow that requires human sign-off before sensitive actions. You'll learn how tool routing works (how the LLM decides which tool to call), how to scope tools so they don't create loops, and how to write tools that fail safely when they encounter unexpected state.

**Topics:**
- Skills architecture: what a skill is, how it's discovered, how it's invoked
- Exec policy: allow/deny rules, approval flows, blast radius scoping
- Tool routing: how the LLM selects tools, disambiguation, fallback behavior
- Writing safe tools: idempotency, error surfacing, side-effect boundaries
