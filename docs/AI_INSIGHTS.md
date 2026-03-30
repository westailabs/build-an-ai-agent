# Project AI Insights (Long-Term Memory)

## Purpose
This document serves as the **Long-Term Memory** for AI agents working on **build-an-ai-agent**. It captures project-specific behavioral nuances, recurring pitfalls, and architectural decisions that are not strictly "rules" (in `AI_DIRECTIVES.md`) but are critical for maintaining continuity.

## 1. Architectural Patterns

- **Artifacts**: Always update `task.md` before tool calls when starting a new phase.
- **Module structure**: Each module is a directory (`00-foundations/` through `09-production/`) containing a `README.md` as the primary content file. Supporting code and configs go in subdirectories within the module (e.g., `01-runtime/configs/`, `05-tools/examples/`).
- **Tone standard**: Engineer-to-engineer, no fluff. Skip the "you'll learn how to..." framing — say what it does. "What We Broke" sections are first-person, past-tense, real failures.
- **Module READMEs**: Production content (00) contains full writeups. Stubs (01-09) follow the format: title, coming-soon marker, prerequisites, 3-4 sentence description, topic bullets.

## 2. Recurring Pitfalls

- **Testing**: Do not assume tests pass; always checking logs.
- **Dependencies**: Check `requirements.txt` before adding new libraries.
- **Stub completeness**: Module stubs must include prerequisites linking to the prior module — this enforces the sequential learning path.

## 3. Workflow Nuances

- **Verification**: Trust the test runner (`pytest` or `verify.yml`) over your assumptions.
- **Content vs. code**: This is a documentation/course repo. The primary deliverable is markdown content, not Python code. The `src/` directory exists from scaffolding but the course content lives in module directories.

## 4. Change Log

### 2026-03-29 — Scaffold Fixes
**Changed:**
- `requirements.txt` — Created (was missing); minimal placeholder comment so CI doesn't fail
- `requirements-dev.txt` — Created (was missing); includes `pytest`, `flake8`, `pre-commit` as used by CI and pre-commit config
- `LICENSE` — Added MIT license, copyright West AI Labs LLC 2026
- `GEMINI.md` — Fixed broken entry point reference from `src/main.py` to `src/build_an_ai_agent/__init__.py`; removed stale Forge Scaffolder attribution
- `docs/feature_template.md` — Replaced `forge --flag` and `dist/` references with generic course-appropriate manual verification steps

**Why:** Forge scaffold generated placeholder references (`src/main.py`, `forge --flag`, `dist/`) that don't exist in this repo. Requirements files were absent, causing CI failures. LICENSE was missing for a public course repo.

### 2026-03-29 — Initial Course Content
**Added:**
- `README.md` — Full course overview: what it is, who it's for, how to use it, module index table, stack table, attribution, MIT license
- `00-foundations/README.md` — Complete module: agent definition, 6 components, stack decisions, ASCII architecture diagram, "What We Broke" (5 real production failures), what you'll build by module 09
- `01-runtime/README.md` through `09-production/README.md` — Stub READMEs with title, prerequisites, description, and topic bullets for each module

**Decisions made:**
- Module stubs link to the prior module's README in their prerequisites section, enforcing sequential flow
- "What We Broke" section in module 00 uses real failure patterns drawn from production experience (compaction/SOUL.md eviction, MEMORY.md staleness, sub-agent exec loops, `doctor --fix` incident, port collision death spiral)
- ASCII diagram uses box-drawing characters that render cleanly in GitHub markdown
- Course tone: engineering manual, not tutorial. No "In this module, you will learn..." framing.
