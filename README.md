# research-os

Personal AI-native research operating system. Optimized for durable knowledge accumulation, investment research, coding, data analysis, and structured cognitive workflows.

**Version:** v3  
**Status:** Active  
**Architecture ref:** personal_ai_research_os_v3.pdf

---

## Core Model

| Component | Role | How It Feels |
|---|---|---|
| Claude Co-Work | Primary operating cockpit | Main workspace |
| GitHub | Canonical machine memory | Structured state |
| Notion | Human reflective memory (deferred) | Readable notebook |
| Cursor | Implementation environment | Engineering workstation |
| Terminal / APIs | Execution and data layer | Reality interface |
| ChatGPT | Independent sparring partner | External challenger |

The objective is not to maximize AI output volume. It is to compound intelligence over time through structured memory, reproducible workflows, adversarial reasoning, and disciplined research practices.

---

## Agent System

Seven cognitive modes, invoked explicitly inside Claude Co-Work.

| Agent | Role | Invocation |
|---|---|---|
| **Atlas** | Research Lead (default) | `[Atlas]:` |
| **Vector** | Quant & Data | `[Vector]:` |
| **Forge** | Systems & Implementation | `[Forge]:` |
| **Beacon** | Investment Strategist | `[Beacon]:` |
| **Sentinel** | Red Team / Critic | `[Sentinel]:` |
| **Archive** | Documentation Steward | `[Archive]:` |
| **Compass** | Legal / Structure | `[Compass]:` |

See `AGENTS.md` for quick reference. See `docs/agents/` for full definitions.

---

## Repo Structure

```
research-os/
├── README.md                    ← this file
├── OPERATING_GUIDE.md           ← how to run a session
├── AGENTS.md                    ← agent quick reference
├── docs/
│   ├── architecture/            ← system design and stack decisions
│   ├── governance/              ← principles, memory rules
│   └── agents/                  ← full agent definitions
├── research/
│   ├── active/                  ← live research threads
│   └── completed/               ← finished research with summaries
├── decisions/
│   ├── adr/                     ← architecture decision records
│   └── investment/              ← investment decision memos
├── logs/
│   └── sessions/                ← session summaries
├── templates/                   ← blank canonical templates
├── code/                        ← scripts, notebooks, tools
└── archive/                     ← deprecated content
```

---

## Key Documents

- `OPERATING_GUIDE.md` — how to start and end a session, load context, invoke agents
- `docs/governance/principles.md` — operating principles
- `docs/governance/memory-rules.md` — what gets promoted, what gets discarded
- `docs/architecture/overview.md` — system architecture

---

## Governance

- Human is final authority
- Sentinel review is mandatory for high-stakes outputs
- Architecture changes require ADRs
- GitHub commits are human-reviewed
- No autonomous execution without explicit approval
