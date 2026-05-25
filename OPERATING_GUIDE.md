# Operating Guide

How to run the research-os system inside Claude Co-Work.

**Last updated:** 2026-05-25  
**Architecture ref:** personal_ai_research_os_v3.pdf

---

## The Operating Model

Claude Co-Work is the primary cockpit. You spend most of your working time here. GitHub, Cursor, terminal, and APIs support the Co-Work workflow — they do not replace it.

Every session is intentional. You arrive with a purpose, work through it with the right agents, and close with a summary that gets committed to GitHub.

---

## Starting a Session

### Step 1 — Load context

At the start of any non-trivial session, paste or reference the relevant context:

**Always load (minimum):**
- This file (`OPERATING_GUIDE.md`) — or summarize the operating model verbally if already familiar
- `AGENTS.md` — agent quick reference

**Load as needed:**
- Relevant active research folder contents
- Prior session summary (`logs/sessions/`)
- Relevant ADRs or investment theses
- Specific agent definitions if the session is specialist-heavy

You do not need to load everything every time. Load what is relevant to today's work.

### Step 2 — State your intent

Open with a clear statement of what this session is for. Example:

> "Starting a research session on [topic]. I want to [specific goal]. Active context: [what you've loaded]."

This sets the operating frame. Atlas picks it up by default.

### Step 3 — Name the active agent

If you are not starting with Atlas, name the agent explicitly at the start:

> `[Forge]: I want to assess the feasibility of...`

---

## During a Session

### Agent invocation

```
[AgentName]: [instruction or question]
```

Default agent is **Atlas** when no agent is named. All transitions must be explicit.

| Agent | Use when you need... |
|---|---|
| `[Atlas]` | Research synthesis, framing, investigation coordination |
| `[Vector]` | Quantitative modeling, data analysis, statistical reasoning |
| `[Forge]` | Systems feasibility, API integration, implementation architecture |
| `[Beacon]` | Investment thesis development, portfolio reasoning |
| `[Sentinel]` | Adversarial review, assumption attacks, blind-spot detection |
| `[Archive]` | Session close, documentation, memory hygiene |
| `[Compass]` | Legal analysis, structural implications, regulatory framing |

### Sentinel review

Mandatory before any high-stakes output leaves the session.

High-stakes outputs include:
- Investment thesis conclusions
- Architecture decisions
- Legal analysis outputs
- Any document that informs a real-world action

Invoke with:
```
[Sentinel: mandatory review]: [paste the output to be reviewed]
```

### Agent handoffs

Explicit. Name the transition:
```
[Sentinel]: Attack the above thesis.
[Atlas: resuming]: Given Sentinel's critique, here is the revised framing...
```

---

## Ending a Session

Every meaningful session ends with a summary.

**Meaningful session** = any session where a decision was made, research was synthesized, code was written, or a direction changed.

### Step 1 — Invoke Archive

```
[Archive]: Close this session and generate a summary.
```

Archive produces a draft summary using the template in `templates/session-summary.md`.

### Step 2 — Review the summary

Read it. Edit it. Make sure:
- Decisions are numbered and clear
- Next actions are specific and owned
- Memory promotion candidates are flagged, not assumed

### Step 3 — Commit to GitHub

Save the summary to `logs/sessions/YYYYMMDD-NNN-short-title.md` and commit:

```
session(YYYYMMDD): session-NNN short title
```

### Step 4 — Promote memory (if flagged)

If Archive flagged memory promotion candidates:
1. Review each candidate
2. Approve or discard
3. Draft the document (Archive can help)
4. Commit to the appropriate folder

---

## Memory Promotion (Quick Reference)

Promote to GitHub when the content is:
- A validated architecture decision
- A completed research conclusion
- A reusable template or workflow
- A governance change
- An investment thesis ready for decision input

Discard when the content is:
- Raw exploration that did not land
- Intermediate reasoning that is captured in the conclusion
- Anything re-derivable in under 10 minutes

See `docs/governance/memory-rules.md` for the full ruleset.

---

## ChatGPT as Sparring Partner

ChatGPT is an independent cognitive layer — a deliberate external challenger, not a substitute for Claude.

**Use ChatGPT for:**
- Independent second opinions on research conclusions
- Alternative framings you want stress-tested
- Adversarial critique after Sentinel review (external sanity check)
- Detecting blind spots in your reasoning
- Prompt refinement and reframing experiments

**Do not use ChatGPT for:**
- Long-running continuity (it has no memory of this system)
- Canonical documentation or governance
- Operational coordination

**Protocol:**
1. Complete the work in Claude Co-Work
2. After Sentinel review, paste the relevant output into ChatGPT with a challenge prompt: "What is wrong with this analysis? What am I missing? What assumptions are most dangerous?"
3. Bring the response back into Co-Work for Atlas or Sentinel to synthesize

---

## Git Commit Conventions

```
type(scope): short description
```

| Type | Use for |
|---|---|
| `docs` | Documentation |
| `research` | Research notes and memos |
| `session` | Session summaries |
| `adr` | Architecture decision records |
| `template` | Templates |
| `code` | Scripts and tools |
| `fix` | Corrections |
| `archive` | Deprecated content |

---

## Naming Conventions

| Item | Format | Example |
|---|---|---|
| Session logs | `YYYYMMDD-NNN-short-title.md` | `20260525-001-repo-structure.md` |
| ADRs | `YYYYMMDD-NNN-short-title.md` | `20260525-001-repo-structure.md` |
| Investment memos | `YYYYMMDD-NNN-short-title.md` | `20260601-001-company-x.md` |
| Research memos | `YYYYMMDD-short-topic.md` | `20260525-fintech-infra.md` |
| Research projects | `YYYYMMDD-short-topic/` (folder) | `20260525-fintech-infra/` |

---

## What This System Is Not

- Not an autonomous agent system
- Not a fragmented multi-chat workflow
- Not a Notion-first environment
- Not optimized for volume or speed of output
- Not a replacement for human judgment on high-stakes decisions
