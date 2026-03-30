# Module 09 — Production

> 🚧 **Coming Soon**

**Prerequisites:** [Module 08 — IaC](../08-iac/README.md) — your infrastructure needs to be codified before you can monitor it properly.

---

## What This Module Covers

We harden the agent stack for real production operation: monitoring that tells you when something is wrong before the agent does something embarrassing, token budget management that keeps API costs predictable, recovery runbooks written from actual incidents, and a root cause analysis (RCA) process for when things go sideways. This module is built almost entirely from real outages and bugs we hit running Moto in production — the failures you'll read about here happened, and the mitigations actually work.

**Topics:**
- Monitoring: service health checks, token budget tracking, alert thresholds
- Recovery runbooks: gateway restart procedure, memory corruption recovery, channel re-pairing
- Token budget management: per-session limits, cron job cost tracking, model tier decisions
- Incident RCA: how to analyze what went wrong, what to write down, what to change
