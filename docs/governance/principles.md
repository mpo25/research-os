# Operating Principles

**Last updated:** 2026-05-25

These are the non-negotiable operating constraints of the system. They exist to prevent predictable failure modes.

---

## The Nine Principles

### 1. Human is final authority
No output — from any agent, any workflow, any analysis — is acted upon without explicit human approval. The system produces inputs to human decisions. It does not make decisions.

### 2. High-stakes outputs require Sentinel review
Before any high-stakes output leaves the session, Sentinel must review it adversarially. This is not optional.

High-stakes outputs include: investment thesis conclusions, architecture decisions, legal analysis, any document intended to inform a real-world action.

### 3. GitHub is canonical state
If it is not in GitHub, it is not the system's state. Working context, session exploration, and Notion views are not canonical. GitHub is.

### 4. Session summaries are mandatory for meaningful sessions
Every meaningful session — one where a decision was made, research was synthesized, code was written, or a direction changed — ends with a committed session summary.

### 5. Memory is selective and anti-rot by design
Default is discard. Content is promoted to GitHub only when it is durable and validated. See `memory-rules.md` for the full ruleset.

### 6. Complexity emerges from validated need
No tool, workflow, automation, or agent capability is added until a real session reveals a clear need for it. Premature complexity is the primary risk.

### 7. One consolidated cockpit
All operational work happens in Claude Co-Work. No fragmentation across parallel chats, disconnected projects, or competing tools.

### 8. Architecture changes require ADRs
Any decision that meaningfully changes how the system works or is built requires an Architecture Decision Record. The rationale for the decision is as important as the decision itself.

### 9. No autonomous execution without explicit approval
Claude agents do not execute actions in the external world (commits, API calls, file modifications) without explicit human approval in the session. Agents produce drafts and recommendations. Humans act on them.

---

## Failure Modes These Principles Prevent

| Principle | Failure mode prevented |
|---|---|
| Human is final authority | Acting on confident-sounding AI output without review |
| Sentinel review | Shipping flawed thesis or decision due to unchallenged assumptions |
| GitHub is canonical | Treating session context or Notion as the source of truth |
| Mandatory summaries | Losing session context and re-deriving work in future sessions |
| Selective memory | Memory bloat, rot, duplication, and noise accumulation |
| Complexity from need | Building infrastructure for workflows that don't exist yet |
| One cockpit | Context fragmentation across parallel environments |
| ADRs for architecture | Reversing decisions without understanding why they were made |
| No autonomous execution | Unreviewed side effects from agent actions |

---

## When Principles Conflict

If following one principle would violate another, escalate to human judgment. The principles are ordered roughly by priority — earlier principles override later ones in cases of genuine conflict.
