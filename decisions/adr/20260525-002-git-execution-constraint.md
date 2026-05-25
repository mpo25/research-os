# ADR-002: Git Operations Run From Local Terminal Only

**Date:** 2026-05-25  
**Status:** Accepted  
**Agents involved:** Forge  
**Sentinel reviewed:** Not applicable

---

## Context

During Phase 2 and Phase 3 setup, git commits were attempted from inside Claude Co-Work's sandbox shell. The first commit in a session succeeds, but subsequent commits fail because the sandbox leaves stale `.git/*.lock` files on the mounted macOS filesystem that cannot be removed from within the sandbox (permission denied at the OS level).

---

## Decision

Claude Co-Work may author, edit, and propose commits for files. All git operations (add, commit, push, branch management) are executed by the human from the local terminal.

Claude's role in git workflow:
- Draft and edit files in the workspace folder
- Propose a commit plan: which files to stage and what commit messages to use
- Help interpret git errors if they arise

Human's role:
- Review `git status --short` before committing
- Stage, commit, and push from terminal
- Clear stale locks if needed: `rm -f .git/HEAD.lock .git/index.lock .git/objects/maintenance.lock`

---

## Alternatives Considered

- **Running all git ops from Co-Work:** Rejected. Sandbox permission constraints make this unreliable after the first commit in a session. The workaround (clearing locks between each commit) is fragile and requires human intervention anyway.

- **Single batched commit per session:** Partially viable but loses the semantic granularity of per-change commits, which matters for audit trail and future navigation of git history.

---

## Consequences

**Positive:**
- Reliable git operations — no lock conflicts
- Human retains full control over what gets committed and when
- Cleaner separation: Claude authors, human commits

**Negative / accepted tradeoffs:**
- Adds a manual step to every session close
- Requires human to have terminal open during commit workflow

---

## Review Trigger

Revisit if Co-Work's sandbox gains the ability to reliably clear its own lock files, or if a future Co-Work version handles git natively without the mounted filesystem constraint.

---

## Commit Reference

`adr(002): git-execution-constraint`
