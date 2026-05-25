# Phase 1 — Foundation Specifications

**Status:** Accepted  
**Date:** 2026-05-25  
**Session:** 001  
**Reference:** personal_ai_research_os_v3.pdf

This document captures the 10 foundational definitions for the research-os system. It is the canonical reference for repo setup and should be committed to GitHub as `docs/architecture/phase-1-foundation.md`.

Designations: **[Required]** = non-negotiable for system to function. **[Recommended]** = add early, skip at cost. **[Optional]** = defer until need is clear.

---

## 1. Repo Structure

**[Required]** Private GitHub repo: `research-os`

```
research-os/
├── README.md
├── OPERATING_GUIDE.md
├── AGENTS.md
├── docs/
│   ├── architecture/
│   │   ├── overview.md
│   │   └── stack.md
│   ├── governance/
│   │   ├── principles.md
│   │   └── memory-rules.md
│   └── agents/
│       ├── atlas.md
│       ├── vector.md
│       ├── forge.md
│       ├── beacon.md
│       ├── sentinel.md
│       ├── archive.md
│       └── compass.md
├── research/
│   ├── active/
│   └── completed/
├── decisions/
│   ├── adr/
│   └── investment/
├── logs/
│   └── sessions/
├── templates/
│   ├── session-summary.md
│   ├── adr.md
│   ├── research-memo.md
│   └── investment-thesis.md
├── code/
└── archive/
```

**Notes:**
- `README.md`, `OPERATING_GUIDE.md`, and `AGENTS.md` live at root because they are loaded into Claude Projects as system context. Burying them in `/docs` increases the chance they drift out of date.
- `code/` is flat initially. Subfolders emerge when 3+ scripts serve the same purpose.
- `archive/` holds deprecated docs and superseded decisions — not a dumping ground.

---

## 2. Folder Purposes

| Folder | Purpose | Required? |
|---|---|---|
| `docs/architecture/` | System design decisions and stack rationale | Required |
| `docs/governance/` | Operating principles, memory rules, review triggers | Required |
| `docs/agents/` | Agent definitions (loaded into Claude Projects) | Required |
| `research/active/` | Live research threads, one subfolder per project | Required |
| `research/completed/` | Completed research with summary doc | Required |
| `decisions/adr/` | Architecture Decision Records | Required |
| `decisions/investment/` | Investment decision memos | Required |
| `logs/sessions/` | Session summaries, one file per session | Required |
| `templates/` | Canonical blank templates | Required |
| `code/` | Scripts, notebooks, analysis tools | Required |
| `archive/` | Deprecated docs, superseded decisions | Recommended |

**Rule:** Nothing goes into `research/completed/` or `decisions/investment/` without a synthesis summary document. Raw notes stay in `active/` or get discarded.

---

## 3. Core Markdown Files

### System documents — load into Claude Projects

| File | Purpose |
|---|---|
| `README.md` | What this repo is, how to orient |
| `OPERATING_GUIDE.md` | How to run a session, agent invocation, workflow |
| `AGENTS.md` | Quick reference: all agents, roles, invocation phrases |

### Reference documents — GitHub only

| File | Purpose |
|---|---|
| `docs/architecture/overview.md` | System architecture narrative |
| `docs/architecture/stack.md` | Tool stack, integration decisions |
| `docs/governance/principles.md` | Core operating principles |
| `docs/governance/memory-rules.md` | What gets promoted, what gets discarded |

### Agent definitions — load into Claude Projects

One file per agent in `docs/agents/`. Structure defined in section 4.

### Operational templates — `templates/`

- `session-summary.md`
- `adr.md`
- `research-memo.md`
- `investment-thesis.md`

---

## 4. Agent Definition Structure

**[Required]** Each agent file in `docs/agents/` follows this schema:

```markdown
# [Agent Name] — [Role Title]

## Identity
One sentence on what this agent is.

## Primary Function
What it does in 2-3 sentences. Concrete, not aspirational.

## Responsibilities
- Specific task 1
- Specific task 2

## Does NOT Do
- Hard boundary 1
- Hard boundary 2

## Typical Invocation
"[AgentName]: [what to ask]"

Examples:
- "[Atlas]: Synthesize the key claims in this research thread."

## Handoff Conventions
- Hands off TO: [agents this one typically feeds]
- Receives FROM: [agents that typically feed into this one]

## Notes
Any important caveats about how this agent should be used.
```

**Note:** The "Does NOT Do" section is the most important. It prevents scope creep between agents and forces explicit agent transitions.

---

## 5. Session Summary Workflow

**[Required]** Every meaningful session ends with a summary committed to `logs/sessions/`.

**Trigger:** Any session where a decision was made, research was synthesized, code was written, or a direction was changed. Pure exploration without conclusions = optional.

**Workflow:**
1. Human signals session end: `[Archive]: close this session`
2. Archive generates summary using the session-summary template
3. Human reviews and edits if needed
4. Human commits to `logs/sessions/YYYYMMDD-NNN-short-title.md`
5. Archive flags items for memory promotion

**What the summary captures:**
- Date, session number, agents active
- Context loaded at start
- Work done (outcomes, not transcript)
- Decisions made (numbered)
- Key learnings
- Open questions
- Proposed memory promotions
- Discard decisions
- Next actions (numbered, owned)

**What it does NOT capture:** raw conversation transcript, speculative tangents that were abandoned, intermediate drafts.

---

## 6. Memory Promotion Rules

**[Required]** Default is discard. Promote only durable, validated content.

### Promote to GitHub when:

| Category | Promote if... |
|---|---|
| Architecture decision | Changes how the system works or is built |
| Validated assumption | Confirmed against evidence, not just stated |
| Research conclusion | Synthesis is complete, not just notes |
| Reusable template | You will use this pattern again |
| Governance change | Changes how you operate |
| Investment thesis | Sufficiently developed to be a decision input |
| ADR | Any meaningful tooling or process change |

### Do NOT promote:
- Raw research notes (keep in `research/active/` until synthesis)
- Speculative exploration that didn't land
- First-draft analyses
- Intermediate reasoning steps
- Anything re-derivable in under 10 minutes

### Promote to Notion when:
- You want a human-readable dashboard view
- It's a summary meant for navigation, not canonical state

**Rule:** If the same fact would need to be in both GitHub and Notion, GitHub is canonical. Notion is a view.

**Promotion process:**
1. Archive flags candidates at session end
2. Human approves or rejects each
3. Archive drafts the document
4. Human commits

---

## 7. ADR Workflow

**[Required]** Architecture Decision Records live in `decisions/adr/`.

**When to write one:** Any decision that would be confusing to reverse-engineer later — why a tool was chosen, why an agent's scope changed, why a workflow was adopted.

**File naming:** `YYYYMMDD-NNN-short-kebab-title.md`  
Example: `20260525-001-repo-structure.md`

**Template:**

```markdown
# ADR-NNN: [Short Title]

**Date:** YYYY-MM-DD
**Status:** Proposed | Accepted | Superseded by ADR-XXX
**Agents involved:** [e.g., Forge, Atlas]

## Context
What situation prompted this decision? What constraints existed?

## Decision
What was decided, stated clearly.

## Alternatives Considered
- Alternative 1 and why rejected
- Alternative 2 and why rejected

## Consequences
- Positive consequences
- Negative consequences / accepted tradeoffs

## Review Trigger
What would make you revisit this decision?
```

**Process:** Forge or Atlas drafts. Sentinel reviews if high-stakes. Human approves and commits.

---

## 8. Git Workflow Conventions

**[Required — simple version]**

Single `main` branch initially. No feature branches until you have 20+ sessions of output.

### Commit message format:

```
type(scope): short description

[optional body]
```

### Commit types:

| Type | Use for |
|---|---|
| `docs` | Documentation additions or updates |
| `research` | Research notes, memos, theses |
| `session` | Session summaries |
| `adr` | Architecture decision records |
| `template` | Template additions or changes |
| `code` | Scripts or analysis code |
| `fix` | Corrections to existing content |
| `archive` | Moving deprecated content |

### Examples:
```
docs(agents): add atlas agent definition
session(20260525): session-001 repo structure design
adr(001): repo structure decision
research(active): initiate thread on fintech infrastructure
```

### Rules:
- Commits are human-reviewed before push (initially)
- No force-pushes to main
- One logical change per commit
- Commit messages: present tense, imperative mood

---

## 9. Naming Conventions

**[Required]**

| Item | Convention | Example |
|---|---|---|
| All files | `kebab-case.md` | `investment-thesis.md` |
| Session logs | `YYYYMMDD-NNN-short-title.md` | `20260525-001-repo-structure.md` |
| ADRs | `YYYYMMDD-NNN-short-title.md` | `20260525-001-repo-structure.md` |
| Investment decisions | `YYYYMMDD-NNN-short-title.md` | `20260601-001-company-x.md` |
| Research memos | `YYYYMMDD-short-topic.md` | `20260525-fintech-infra.md` |
| Research projects | `research/active/YYYYMMDD-short-topic/` | `research/active/20260525-fintech-infra/` |
| Agent files | `{agentname}.md` (lowercase) | `atlas.md` |

**Counter format:** `NNN` = zero-padded 3-digit integer, per sequence type (ADR and session counters are independent). Start at `001`.

**Agent naming:**
- In filenames: lowercase (`atlas.md`)
- In text: capitalized (`Atlas`)
- In invocations: bracketed (`[Atlas]`)

---

## 10. Agent Invocation Conventions

**[Required]** Explicit invocation only. Never drift into an agent's mode without naming it.

### Standard invocation:
```
[AgentName]: [instruction or question]
```

### Examples:
```
[Atlas]: Synthesize the key structural risks in this regulatory filing.
[Vector]: Model revenue sensitivity to these three assumptions.
[Forge]: Assess the feasibility of this API integration approach.
[Beacon]: What does this data imply for our investment thesis on X?
[Sentinel]: Attack the above analysis. What are we missing or assuming?
[Archive]: Close this session and generate a summary.
[Compass]: What are the structural and legal implications of this arrangement?
```

### Rules:
- Default agent is Atlas when no agent is named
- Agents do not auto-switch — transitions must be explicit
- One agent active at a time per response
- Sentinel review is mandatory before any high-stakes output leaves the system

### Special invocations:
```
[Sentinel: mandatory review]  — high-stakes output requiring adversarial pass
[Atlas: resuming]             — returning to default after a specialist agent
[Archive]: close this session — triggers session summary workflow
```

### High-stakes outputs (Sentinel required):
- Investment thesis conclusions
- Architecture decisions
- Legal analysis outputs
- Any document intended to inform a real-world action

---

## Open Questions

These require resolution before Phase 2 begins:

1. **Claude Projects vs. Co-Work:** Are you planning to use a dedicated Claude Project (with uploaded system docs) or continuing in Co-Work? This affects which files need to be formatted for upload vs. inline loading.

2. **GitHub setup:** Is the `research-os` repo already created, or does it need to be initialized as part of Phase 2?

3. **Notion:** Do you have a Notion workspace designated for this system, or is that deferred?

---

*Generated: 2026-05-25 | Session 001 | Agents: Forge, Atlas*
