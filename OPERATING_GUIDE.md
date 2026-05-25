# Operating Guide

How to run the research-os system inside Claude Co-Work.

**Last updated:** 2026-05-25 (Phase 3)  
**Architecture ref:** personal_ai_research_os_v3.pdf

---

## The Operating Model

Claude Co-Work is the primary cockpit. You spend most of your working time here. GitHub, Cursor, terminal, and APIs support the Co-Work workflow — they do not replace it.

Every session is intentional. You arrive with a purpose, work through it with the right agents, and close with a summary that gets committed to GitHub.

---

## Starting a Session

### Step 1 — Open with the session header

Paste this block at the start of every non-trivial session. It restores state and sets the operating frame.

```
--- SESSION START ---
Date: YYYY-MM-DD
Session: NNN
Intent: [one sentence — what this session is for]

Context loaded:
- Prior session: logs/sessions/YYYYMMDD-NNN-title.md [paste or summarize below]
- Active research: [paste relevant files or summarize]
- [any other specific docs]

Active agent: Atlas
--- BEGIN ---
```

Paste the prior session summary inline if it's short. Summarize it if it's long. The intent line is the most important part — it anchors the entire session.

### Step 2 — Context loading by tier

Load only what is relevant. Start lean and pull in more as needed.

**Tier 1 — Internalized (do not paste unless specifically relevant):**
- OPERATING_GUIDE — you know the system after a few sessions
- AGENTS.md — agent model is internalized
- Governance principles — operating constraints are internalized

**Tier 2 — Load per session based on intent:**
- Prior session summary — always load if continuing prior work
- Active research files — paste the specific files relevant to today's question
- Relevant ADRs — only if architecture is being discussed
- Active investment theses — only if thesis work is the focus

**Tier 3 — Load on demand mid-session:**
- Specific agent definitions — if a specialist needs precise scope
- Completed research — if referencing a prior conclusion
- Templates — when Archive is drafting a document

**Rule:** Start with the prior session summary and your intent. Let the session reveal what else is needed.

### Step 3 — Name the active agent

Atlas is default. If starting with a specialist, name it immediately:

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

Mandatory before any high-stakes output leaves the session. If you're asking whether Sentinel is needed, it probably is.

High-stakes outputs requiring mandatory review:
- Investment thesis conclusions (any Beacon output reaching conclusion status)
- Architecture decisions (before committing an ADR)
- Legal analysis intended to inform a real-world action
- Any document that directly informs a real-world decision

```
[Sentinel: mandatory review]: [paste the specific output — not the whole session]
```

After Sentinel, the sequence is fixed: critique → revision (by Atlas or Beacon) → human review → decision. Sentinel without revision is noise.

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

Archive produces a draft using `templates/session-summary.md` and lists memory promotion candidates with a recommendation for each.

### Step 2 — Review the summary

Edit Archive's draft. Check specifically:
- Decisions: numbered and unambiguous?
- Next actions: specific enough to be actionable next session?
- Promotion candidates: do you agree with Archive's recommendations?

Do not commit Archive's draft verbatim. You make it accurate.

### Step 3 — Commit to GitHub

```
git add logs/sessions/YYYYMMDD-NNN-short-title.md
git commit -m "session(YYYYMMDD): session-NNN short-title"
```

### Step 4 — Promote memory (if flagged)

For each promotion candidate:
- Approve → Archive drafts the document → human commits
- Discard → note why in the session summary's discard section

### Step 5 — After Sentinel: ChatGPT check (optional, high-stakes only)

For outputs that went through mandatory Sentinel review and will inform a real-world action, consider an external ChatGPT challenge before closing. See ChatGPT protocol below.

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
1. Complete the work in Co-Work, including Sentinel review and revision
2. Extract only the output — strip agent names and internal session structure
3. Open ChatGPT with a challenge frame, not a request for validation:
   - "What is wrong with this analysis? Be specific about the weakest assumptions."
   - "Give me an alternative framing that would lead to the opposite conclusion."
   - "What is this analysis failing to consider?"
4. Bring the response back into Co-Work:
   ```
   [Atlas]: ChatGPT offered this critique: [paste]. Integrate what's useful, flag what to discard.
   ```
5. Human decides what to incorporate

**What ChatGPT is not:** a second vote. Agreement from ChatGPT does not strengthen the thesis — it may just mean you prompted toward confirmation. The value is disagreement and alternative framing.

---

## Git Workflow

### Execution constraint

Claude authors and edits files. All git operations run from your local terminal.

Co-Work leaves stale `.git/*.lock` files on the mounted macOS filesystem after the first commit in a session, causing subsequent git commands to fail. Do not run git commands inside Co-Work.

**Standard workflow:**
1. Claude drafts or edits files
2. Claude proposes a commit plan (files + messages)
3. Human reviews `git status --short`
4. Human stages, commits, and pushes from terminal

**If a stale lock appears:**
```bash
rm -f .git/HEAD.lock .git/index.lock .git/objects/maintenance.lock
```

### Commit conventions

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
