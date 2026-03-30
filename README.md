# Build an AI Agent — A Production Course

**Not a tutorial. A field manual.**

This course is built from 45+ days of real production experience running a production OpenClaw agent on Ubuntu 24.04 bare metal. Every lesson here came from something that actually broke, shipped, or surprised us in production.

---

## Who This Is For

Engineers who want to run a real agent in production — not just call an API.

If you've already done the "build a chatbot in 30 minutes" tutorial and wondered *"okay but how do I actually run this thing?"* — this is for you.

**You should already know:**
- Basic Linux (systemd, file permissions, SSH)
- Python (enough to read it)
- Git
- Docker (helpful but not required)

**You don't need:**
- Machine learning knowledge
- Cloud infrastructure
- Prior agent experience

---

## What You'll Build

By the end of this course, you'll have a fully operational production agent with:
- A persistent identity and memory system
- Telegram and/or Discord as communication channels
- A tool/skills execution layer with approval flows
- Scheduled autonomous work (cron + heartbeat)
- Infrastructure-as-code (Ansible) for reproducible deploys
- A monitoring setup and recovery runbook you actually wrote

---

## How to Use This Course

Work through modules **00 through 09 in order**. Each module builds on the previous one. Skipping ahead works, but you'll hit gaps.

Every module has:
- A `README.md` explaining concepts
- Working code and configs in the module directory
- A "What We Broke" section — real production lessons, not sanitized examples

```
00-foundations/    ← Start here
01-runtime/
02-channels/
03-memory/
04-rag/
05-tools/
06-cron/
07-multi-agent/
08-iac/
09-production/
```

---

## Module Index

| Module | Name | What It Covers |
|--------|------|----------------|
| [00](./00-foundations/) | **Foundations** | What is an agent, the 6 required components, stack decisions, mental model |
| [01](./01-runtime/) | **Runtime** | OpenClaw setup, systemd service, bare metal vs container tradeoffs |
| [02](./02-channels/) | **Channels** | Telegram + Discord wiring, bot tokens, pairing, message routing |
| [03](./03-memory/) | **Memory** | Session state, daily logs, MEMORY.md pattern, compaction behavior |
| [04](./04-rag/) | **RAG** | pgvector setup, embedding pipelines, query patterns, shared-rag architecture |
| [05](./05-tools/) | **Tools** | Skills system, tool routing, exec policy, approval flows |
| [06](./06-cron/) | **Cron** | Heartbeat design, scheduled jobs, autonomous work loops |
| [07](./07-multi-agent/) | **Multi-Agent** | Agent-to-agent comms, orchestrator pattern, sub-agent delegation |
| [08](./08-iac/) | **IaC** | Ansible-first infrastructure, Gitflow-lite, change control |
| [09](./09-production/) | **Production** | Monitoring, recovery runbooks, token budgets, incident RCA |

---

## The Stack

This course uses [OpenClaw](https://github.com/westailabs) as the agent runtime. It's not the only option, but it's the one we've actually run in production for months. The concepts transfer to any runtime — the patterns are what matter.

| Layer | Tool |
|-------|------|
| Agent Runtime | OpenClaw |
| LLM | Anthropic Claude (claude-sonnet-4-x) |
| Channels | Telegram Bot API, Discord |
| Memory | Markdown files + pgvector (RAG) |
| Scheduling | cron + OpenClaw heartbeat |
| IaC | Ansible |
| Platform | Ubuntu 24.04 bare metal |

---

## Attribution

Built by **West AI Labs** from real production experience.

- GitHub: [https://github.com/westailabs](https://github.com/westailabs)
- Agent: your-agent-name (OpenClaw, your-machine)

---

## License

MIT. Use it, fork it, build on it.

---

*If something is wrong or out of date, open an issue. We run this stuff daily — we'll know.*
