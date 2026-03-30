# Module 00 — Foundations

**Prerequisites:** None. This is the starting point.

---

## What Is an Agent? (The Real Definition)

Forget the hype. Here's the engineering definition:

> **An agent is an LLM + memory + tools + a loop.**

That's it. The LLM produces text. Memory gives it context that persists across calls. Tools let it take actions in the real world. The loop runs it repeatedly so it can do work over time.

Everything else — the product names, the frameworks, the LinkedIn posts — is packaging around those four things.

The loop is what separates an agent from a chatbot. A chatbot responds once and forgets you. An agent can wake up at 3am, check your email, file an issue, update a doc, and go back to sleep — without you touching it.

---

## The 6 Components Every Production Agent Needs

You can get something running with just the LLM and a loop. You cannot run it in production that way. Production requires all six:

### 1. Model
The LLM itself. In this course we use Claude (Anthropic), but the patterns apply anywhere. Key decisions:
- Which model tier? (cost vs. capability)
- Context window size — this directly affects memory architecture
- Rate limits and token budgets

### 2. Channels
How the agent communicates with humans. A model with no channel is a library, not an agent. Channels include:
- Telegram (great for personal/team agents — bot API is excellent)
- Discord (good for community-facing agents)
- Slack, email, SMS — same patterns, different auth

Channels are also where you **receive work** from users, not just send output.

### 3. Memory
This is where most tutorials fall apart. There are four memory tiers:

| Tier | What | Lifespan |
|------|------|----------|
| 0 — Identity | SOUL.md, rules, persona | Permanent |
| 1 — Long-term | Curated facts, decisions | Months |
| 2 — Working | Session state, current context | Hours/days |
| 3 — Ephemeral | Daily logs, in-flight work | Days |

You need all four. Most tutorials only give you ephemeral.

### 4. Tools
How the agent takes actions. Tools are functions the LLM can call: read a file, send a message, run a shell command, query a database. The tool ecosystem is what makes an agent useful vs. just eloquent.

Tool design matters: badly scoped tools cause loops, infinite approvals, or security holes.

### 5. Scheduler
How the agent does work without being asked. This is the difference between a reactive assistant and an autonomous agent. You need:
- A heartbeat (periodic check-in, does the agent have anything to do?)
- Cron jobs (scheduled tasks at specific times)
- Triggered tasks (run when X happens)

### 6. Identity
Who the agent is. This isn't optional or soft — it's load-bearing infrastructure. The agent's identity controls:
- What it will and won't do
- How it communicates
- What it prioritizes when things conflict

Without a written identity (`SOUL.md`, `IDENTITY.md`), the agent drifts. Every time the context window compacts, it loses a little more of itself. A written identity file is the anchor.

---

## Stack Decisions

### Why OpenClaw?
We're not being dogmatic about it. OpenClaw is the runtime we've actually run in production for months. It ships with channels, memory architecture, a skills system, and a heartbeat out of the box. That's months of scaffolding you don't have to write.

The alternative is building all of this yourself, which is educational but not fast. We'll use OpenClaw to go fast, and explain the internals as we go.

### Why Telegram + Discord?
- **Telegram:** The Bot API is clean, well-documented, and doesn't require you to run a server. Polling works fine for personal/small-team agents. Inline buttons, file transfers, reactions — it's a solid channel.
- **Discord:** Better for community/public-facing agents. More complex auth, more powerful features.

Both have mobile apps, which means your agent is accessible from your phone. That matters more than you'd think.

### Why Local-First?
Three reasons:

1. **Cost.** Running a model on Anthropic's API isn't cheap at scale. Running the agent runtime on a $50/month VPS or bare metal you already own is cheap.
2. **Control.** Your agent has access to your files, your calendar, your services. You don't want that on someone else's infrastructure.
3. **Latency.** Local agents respond faster and don't go down when a cloud provider has an outage.

### Why Ansible?
Because "I'll remember how I set this up" is how you end up spending a Saturday rebuilding from scratch.

Ansible gives you reproducible infrastructure. If your server dies, you run a playbook and it's back. If you want to add a second agent, you run the playbook again. It also forces you to document what you actually deployed, which is a gift to future-you.

---

## Mental Model: How Components Connect

```
                        ┌─────────────────────────────────────┐
                        │           SCHEDULER                  │
                        │   (cron jobs, heartbeat loop)        │
                        └──────────────┬──────────────────────┘
                                       │ triggers
                                       ▼
┌──────────────┐     messages    ┌──────────────────────────────┐
│   CHANNELS   │ ◄───────────── │                              │
│  (Telegram,  │                │         AGENT LOOP           │
│   Discord)   │ ──────────────►│   (receive → think → act)   │
└──────────────┘     input      │                              │
                                └───┬──────────────┬───────────┘
                                    │              │
                             reads  │              │  calls
                                    ▼              ▼
                        ┌───────────────┐  ┌──────────────────┐
                        │    MEMORY     │  │     TOOLS        │
                        │  (SOUL.md,    │  │  (exec, search,  │
                        │  MEMORY.md,   │  │   file ops,      │
                        │  session      │  │   API calls)     │
                        │  state, RAG)  │  └──────────────────┘
                        └───────────────┘
                                    │
                             anchors │
                                    ▼
                        ┌───────────────────┐
                        │    IDENTITY       │
                        │  (SOUL.md,        │
                        │   IDENTITY.md,    │
                        │   rules)          │
                        └───────────────────┘

                        ┌───────────────────┐
                        │      MODEL        │
                        │  (LLM API —       │
                        │   Claude, GPT,    │
                        │   local)          │
                        └───────────────────┘
                          (everything routes
                           through the model)
```

The loop sits in the center. Everything else feeds it or is fed by it.

---

## What We Broke

These are real production failures from running Moto (our agent) on `shurtugal-lnx`. Read these now. You will hit variations of all of them.

### 1. Compaction kills safety instructions

**What happened:** As conversations get long, the LLM context window fills up. OpenClaw (and most runtimes) compact the oldest messages to make room. `SOUL.md` — the agent's identity and behavioral rules — lives in the system prompt at the top of context. When compaction is aggressive, it gets evicted.

**Result:** The agent lost its constraints. Started doing things it was explicitly told not to do. Not dramatically — just subtle drift. Approving things it shouldn't. Being less careful.

**Fix:** Your identity files must be re-injected into every session, not just the first message. Treat them as headers, not history.

### 2. Memory goes stale — MEMORY.md is a cache, docs are the source of truth

**What happened:** We updated a workflow doc. The agent's `MEMORY.md` had an older version of the workflow. The agent followed the cached version, not the updated doc.

**Result:** Wrong behavior, hours of debugging, one angry engineer.

**Fix:** Rule in our `AGENTS.md`: "When answering any question about engineering standards — read the relevant doc first, then answer." Memory is a cache. It goes stale. The doc is the authority.

### 3. Sub-agents need delegation rules or they loop on shell execution

**What happened:** A sub-agent was given a vague task: "fix the tests." It had exec access. It ran tests, saw failures, tried to fix them, ran tests again, saw different failures, tried to fix those. Loop.

**Result:** 47 minutes of compute, a file system that looked like it had been through a blender, and tests that were somehow worse.

**Fix:** Delegation rules are mandatory. Sub-agents must have: explicit scope, explicit stop conditions, and exec policy that limits blast radius. "Fix the tests" is not a task spec. "Run `pytest`, read the first failure, write a fix for that one failure, stop" is a task spec.

### 4. `doctor --fix` can introduce breaking changes — read the changelog first

**What happened:** Ran `openclaw doctor --fix` to address some warnings after an upstream sync. It "fixed" config values that our setup had intentionally set differently. Broke the gateway.

**Result:** Gateway outage, 40+ minutes of debugging.

**Fix:** `doctor --fix` is not safe to run blindly after upstream changes. Read the changelog. Know what it's going to touch. Run `doctor` without `--fix` first, review the output, then decide.

### 5. Two agents on one port = death spiral

**What happened:** Testing a second agent config. Both pointed at port 3000. They fought over the port, both crashed, systemd restarted both, they fought again.

**Result:** A crash loop that didn't stop until we manually killed both processes.

**Fix:** One port per agent. No exceptions. Ports go in Ansible inventory so they're tracked. Config collision is a platform-level problem, not something you debug at 11pm.

---

## What You'll Have Built by Module 09

By the time you finish this course:

- **A running agent** on real infrastructure (bare metal or VPS) with a systemd service that restarts on failure
- **Two active channels** — your agent will talk to you on Telegram and/or Discord
- **A 4-tier memory system** — identity, long-term, working, ephemeral
- **A RAG pipeline** — pgvector + embeddings, so your agent can search its own knowledge base
- **A skills/tools layer** — with exec policy and approval flows
- **Scheduled autonomous work** — heartbeat + cron jobs that do real things
- **A multi-agent setup** — at least one orchestrator→sub-agent flow
- **Ansible playbooks** for everything — reproducible from scratch
- **A monitoring setup and recovery runbook** — so when it breaks (it will), you know what to do

That's a production agent. Not a demo, not a prototype — the real thing.

---

## Next: [Module 01 — Runtime](../01-runtime/README.md)

We'll install OpenClaw, wire up a systemd service, and make your first agent respond to a message.
