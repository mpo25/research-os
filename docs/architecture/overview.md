# System Architecture — Overview

**Version:** v3  
**Last updated:** 2026-05-25  
**Reference:** personal_ai_research_os_v3.pdf

---

## Purpose

This system is a personal cognitive operating environment, not a production software system. The objective is to compound intelligence over time through structured memory, reproducible workflows, adversarial reasoning, and disciplined research practices.

The design optimizes for: clarity, continuity, maintainability, reproducibility, and durable deep work. It does not optimize for automation, volume, or speed.

---

## Core Architectural Principles

1. Claude Co-Work is the primary cockpit and operating environment
2. GitHub is canonical machine memory — the source of truth
3. Notion is human-readable reflective memory — a view, not state
4. ChatGPT is an independent cognitive sparring partner — external challenger only
5. Agents are operational cognitive modes, not autonomous AI workers
6. High-stakes outputs require mandatory adversarial review (Sentinel)
7. Memory is selective and anti-rot by design — default is discard
8. The system prioritizes reproducibility and traceability
9. Complexity must emerge from validated need, not speculation

---

## Operating Model

```
Claude Co-Work
    └── Atlas (default)
        ├── Vector (data/quant tasks)
        ├── Forge (systems/implementation tasks)
        ├── Beacon (investment thesis tasks)
        ├── Sentinel (mandatory review gate)
        ├── Compass (legal/structural tasks)
        └── Archive (session close)
              ├── → GitHub (machine memory commit)
              └── → Notion (human-readable summary, deferred)
```

The human is at the top of every decision chain. No output from Sentinel or any agent is acted upon without explicit human approval.

---

## Memory Architecture

| Layer | Lives Where | Purpose | Default |
|---|---|---|---|
| Machine memory | GitHub | Canonical structured state | Persisted |
| Human memory | Notion (deferred) | Readable reflection and navigation | Deferred |
| Working context | Claude Co-Work session | Current reasoning and active context | Ephemeral |
| Ephemeral exploration | Session only | Reasoning steps, drafts, tangents | Discarded |

**Key rule:** Only durable, validated content is promoted to GitHub. Everything else is discarded by default. The promotion decision is made at session close by Archive, reviewed by the human.

---

## Session Lifecycle

```
Session Start
    → Load context (OPERATING_GUIDE + relevant docs)
    → State session intent
    → Name active agent

During Session
    → Explicit agent invocations
    → Explicit agent transitions
    → Sentinel review for high-stakes outputs

Session End
    → [Archive]: close this session
    → Archive generates summary draft
    → Human reviews and edits
    → Commit to logs/sessions/
    → Promote memory (if flagged)
    → Commit promoted memory to GitHub
```

---

## Cognitive Layer Division

| Use Claude For | Use ChatGPT For |
|---|---|
| Long-running project continuity | Independent second opinions |
| Architecture and workflow design | Alternative framings |
| Structured research synthesis | Adversarial critique |
| Memory-aware reasoning | Detecting blind spots |
| Operational coordination | Prompt refinement |

Claude and ChatGPT are not interchangeable. Claude is the primary operating environment. ChatGPT is a deliberate independent review layer invoked after Sentinel for external sanity-checking.

---

## Current System State

- **Phase 1:** Foundation specifications — complete (2026-05-25)
- **Phase 2:** Initial files — in progress
- **Phase 3:** Operational workflow — pending
- **Phase 4:** Tooling decisions — deferred

---

## Evolution Principle

This system evolves through real usage patterns, not speculative design. Friction encountered in the first 5-10 real sessions drives iteration. No complexity is added without a clear trigger from observed need.
