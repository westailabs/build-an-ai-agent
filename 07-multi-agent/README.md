# Module 07 — Multi-Agent

> 🚧 **Coming Soon**

**Prerequisites:** [Module 06 — Cron](../06-cron/README.md) — you should have one solid agent running autonomously before you start orchestrating multiple agents.

---

## What This Module Covers

We build agent-to-agent communication and the orchestrator pattern: a main agent that delegates work to sub-agents with narrowly scoped tasks and clear stop conditions. You'll implement the delegation protocol (how to spawn a sub-agent, what context to pass, how to receive results), learn what goes wrong when sub-agents have too much autonomy (they loop), and build a production-safe pattern for parallel work without losing control of the outcome.

**Topics:**
- Orchestrator pattern: main agent as coordinator, sub-agents as executors
- Delegation protocol: task spec format, context handoff, result collection
- Sub-agent scope control: exec limits, stop conditions, timeout handling
- Failure modes: loops, context drift, conflicting writes — and how to prevent them
