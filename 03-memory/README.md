# Module 03 — Memory

> 🚧 **Coming Soon**

**Prerequisites:** [Module 02 — Channels](../02-channels/README.md) — agent must be receiving messages before memory patterns are meaningful.

---

## What This Module Covers

We build the 4-tier memory architecture that keeps your agent coherent across sessions, restarts, and context compaction. You'll implement session state (JSON), daily logs (append-only markdown), the MEMORY.md pattern (curated long-term memory), and learn how compaction works and why it will bite you if you don't design around it. This module is where the agent stops being a stateless responder and starts being a persistent entity.

**Topics:**
- Session state JSON: what goes in, when it's written, how it's read back
- Daily log pattern: ephemeral append-only records for debugging and review
- MEMORY.md as curated cache: update triggers, staleness risks, compaction strategy
- Identity file anchoring: keeping SOUL.md in context after compaction events
