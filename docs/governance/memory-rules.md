# Memory Rules

**Last updated:** 2026-05-25

The anti-rot ruleset. Default is discard. Promote only durable, validated content.

---

## The Default

Discard. Everything produced in a session is ephemeral unless explicitly promoted.

Exploration, intermediate reasoning, draft analyses, tangents that did not land, and raw notes are not promoted. They served their purpose in the session.

---

## What Gets Promoted to GitHub

| Category | Promote when... | Goes to |
|---|---|---|
| Architecture decision | Changes how the system works or is built | `decisions/adr/` |
| Research conclusion | Synthesis is complete; not just notes | `research/completed/` |
| Investment thesis | Developed enough to be a decision input | `decisions/investment/` |
| Validated assumption | Confirmed against evidence, not just stated | Relevant research folder |
| Reusable template | Will be used again with predictable frequency | `templates/` |
| Governance change | Changes how the system operates | `docs/governance/` |
| Session summary | Meaningful session (see definition below) | `logs/sessions/` |
| Agent definition change | Changes how an agent behaves or is invoked | `docs/agents/` |

---

## What Does NOT Get Promoted

- Raw research notes or source material (keep in `research/active/` until synthesis is complete)
- Speculative exploration that did not produce a conclusion
- First-draft analyses that were superseded
- Intermediate reasoning steps (capture the conclusion, not the path)
- Anything re-derivable from available sources in under 10 minutes
- Session transcripts or conversation logs

---

## The Promotion Process

Archive runs this process at session close.

1. Archive reviews the session and flags promotion candidates
2. Human reviews each candidate and approves or discards
3. For approved candidates, Archive drafts the document using the relevant template
4. Human reviews and edits the draft
5. Human commits to GitHub

Archive proposes. Human decides. Human commits.

---

## What Counts as a Meaningful Session

A session is meaningful (requiring summary + promotion review) if any of the following occurred:

- A decision was made
- Research was synthesized (not just explored)
- Code was written or significantly modified
- A direction was changed
- A new investment thesis was initiated or materially updated
- An architecture decision was made

A session is not meaningful (summary optional) if:
- Only exploration occurred with no conclusions
- The session was abandoned mid-way
- The outputs are all in the "do not promote" category

---

## Memory Promotion to Notion (Deferred)

Notion is not currently in scope. When activated, the model is:

- Notion receives human-readable summaries from GitHub, not raw commits
- Notion content flows: GitHub → Notion, never Notion → GitHub
- Notion is a reflective view, not canonical state

---

## Anti-Rot Rules

1. **No duplicates.** Before promoting, check if an existing document covers the same content. Update it rather than creating a new one.
2. **Supersede explicitly.** If a new ADR or decision supersedes an old one, mark the old one as superseded with a reference to the new one.
3. **Archive, do not delete.** Deprecated content moves to `archive/` — it is not deleted. Git history is the record.
4. **Date everything.** Every promoted document includes a "Last updated" date.
5. **Lean documents.** If a document has not been referenced in 3+ months and does not seem durable, flag it for archiving at the next governance review.

---

## Memory Layers Quick Reference

| Layer | Where | Canonical? | Persisted? |
|---|---|---|---|
| Machine memory | GitHub | Yes | Yes |
| Human memory | Notion (deferred) | No | Yes |
| Working context | Claude Co-Work session | No | No (ephemeral) |
| Ephemeral exploration | Session only | No | No — discarded |
