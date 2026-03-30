# Module 06 — Cron

> 🚧 **Coming Soon**

**Prerequisites:** [Module 05 — Tools](../05-tools/README.md) — autonomous work requires a working tool layer to do anything useful.

---

## What This Module Covers

We make the agent work without being asked. You'll design and implement a heartbeat system (periodic check-ins where the agent decides if it has anything to do), wire up cron jobs for scheduled tasks, and build autonomous work loops that handle real recurring work like email triage, doc maintenance, and status reports. We cover the design constraints that keep autonomous agents from causing chaos: absolute paths, channel ID vs. user ID, quiet hours, and blast radius limits.

**Topics:**
- Heartbeat design: check-in frequency, task detection, stay-quiet logic
- Cron job patterns: one-shot reminders, recurring reports, maintenance tasks
- Autonomous work loop safety: path rules, channel targeting, output rate limiting
- Monitoring autonomous work: what ran, what it did, what it skipped and why
