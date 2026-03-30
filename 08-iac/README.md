# Module 08 — Infrastructure as Code

> 🚧 **Coming Soon**

**Prerequisites:** [Module 07 — Multi-Agent](../07-multi-agent/README.md) — your full agent stack should be working before you codify it into Ansible.

---

## What This Module Covers

We write the Ansible playbooks that let you deploy or rebuild your entire agent stack from scratch in one command. You'll structure roles for the agent runtime, channels, memory infrastructure, and monitoring — and adopt the Gitflow-lite branching model and change control workflow that keeps infrastructure changes safe and auditable. By the end, your production setup is fully documented in code, not just in your head.

**Topics:**
- Ansible role structure for agent infrastructure: runtime, services, config
- Idempotent playbooks: safe to run repeatedly, no surprises on re-run
- Gitflow-lite for IaC: main/develop branches, feature flags, change review
- Change control: what requires a PR, what requires a human sign-off, rollback procedures
