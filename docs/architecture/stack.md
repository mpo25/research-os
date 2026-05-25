# System Stack

**Last updated:** 2026-05-25

Tool-by-tool rationale and integration decisions.

---

## Stack Summary

| Tool | Role | Status | Priority |
|---|---|---|---|
| Claude Co-Work | Primary cockpit | Active | Essential |
| GitHub | Canonical machine memory | Active | Essential |
| Cursor | Implementation environment | Active | Essential |
| Terminal / APIs | Execution layer | Active | Essential |
| ChatGPT | Independent sparring partner | Active | Essential |
| Notion | Human reflective memory | Deferred | Recommended |
| Google Drive | Reference material | As needed | Optional |
| Google Sheets | Quick modeling | As needed | Optional |
| Slack | Collaboration | Deprioritized | Avoid initially |
| Advanced orchestration | Agent automation | Not in scope | Avoid |

---

## Claude Co-Work

**Role:** Primary operating cockpit.  
**Why:** One consolidated environment that maintains session context, supports iterative collaboration, and integrates file access without forcing fragmented chats. Co-Work allows Claude to function as a true operating environment rather than a stateless question-answering tool.  
**Integration model:** Co-Work is the session layer. GitHub is where durable outputs go. Context flows into Co-Work at session start from GitHub-committed documents.  
**Current limitation:** Context must be loaded manually at session start. This is acceptable for now. If it becomes friction, stable documents can be migrated to a Claude Project for persistent preloading.

---

## GitHub

**Role:** Canonical machine memory.  
**Why:** Version-controlled, traceable, structured, and durable. The only reliable way to maintain long-term continuity of system state across sessions. Git history provides auditability.  
**Integration model:** All durable outputs — architecture decisions, research conclusions, session summaries, templates, agent definitions, governance docs — are committed here. Nothing is canonical unless it is in GitHub.  
**Workflow:** Human-reviewed commits. Single `main` branch initially. ADRs for architecture changes.  
**Repo:** `research-os` (private)

---

## Cursor

**Role:** Implementation environment.  
**Why:** Code editor with strong AI integration for implementation work — scripts, notebooks, data pipelines, API clients. Forge operates here for engineering-heavy tasks.  
**Integration model:** Code produced in Cursor is committed to `code/` in the GitHub repo. Cursor does not replace Claude Co-Work for research or strategy work.

---

## Terminal / APIs

**Role:** Execution and data layer.  
**Why:** Reality interface. Data fetching, script execution, API calls, system operations. This is where quantitative models run and data is retrieved.  
**Integration model:** Terminal operations produce outputs that are analyzed in Claude Co-Work (Vector, Forge). Results are synthesized, not raw-pasted into the repo.

---

## ChatGPT

**Role:** Independent cognitive sparring partner.  
**Why:** Deliberate separation of primary cognitive environment (Claude) and external challenger (ChatGPT). Using only one AI system creates blind spots and confirmation loops. ChatGPT provides independent second opinions without memory of the Claude operating context.  
**Integration model:** Used after Sentinel review for external challenge. Outputs are brought back into Claude Co-Work for synthesis. ChatGPT does not have access to the GitHub repo and does not maintain continuity.  
**Usage protocol:** See `OPERATING_GUIDE.md`.

---

## Notion (Deferred)

**Role:** Human reflective memory.  
**Why:** GitHub is the canonical state but not human-readable for navigation and reflection. Notion provides dashboards, readable summaries, and a human-oriented review surface.  
**Status:** Deferred until real usage reveals which reflective views are genuinely useful.  
**Integration model (planned):** Notion will mirror, not duplicate, GitHub content. It will not be used for canonical operational state. Content flows: GitHub → Notion (human-readable view), not Notion → GitHub.  
**Do not:** Use Notion as a primary drafting or research environment. Do not treat Notion content as canonical.

---

## Deprioritized / Out of Scope

**Slack:** Risk of fragmentation, operational noise, and duplicated context. Deprioritized until a specific use case emerges that cannot be served by the existing stack.

**Advanced orchestration (LangChain, AutoGPT, etc.):** Not in scope. Agents are cognitive modes inside Claude, not autonomous systems. Orchestration complexity is added only if a validated need emerges from real usage.

**Zapier / n8n / automation:** Deferred. No automation until workflows are stable enough to be worth automating.
