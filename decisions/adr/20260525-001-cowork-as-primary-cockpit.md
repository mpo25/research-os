# ADR-001: Claude Co-Work as Primary Operating Cockpit

**Date:** 2026-05-25  
**Status:** Accepted  
**Agents involved:** Atlas, Forge  
**Sentinel reviewed:** Not applicable — system design decision, no high-stakes external action implied

---

## Context

During Phase 1 and Phase 2 setup, a decision was required on how to structure the primary operating environment. Two viable paths existed:

1. Claude Projects — with stable architectural documents preloaded as persistent system context, allowing Claude to have those documents available without manual loading each session
2. Claude Co-Work — a single consolidated environment with file access and iterative session-based collaboration, where context is loaded manually at session start

The choice has implications for: how context is managed, how the system evolves, how fragmentation risk is mitigated, and what the primary operational experience feels like.

---

## Decision

Claude Co-Work is the primary operating cockpit. Stable architectural and governance documents live in GitHub as canonical state, and are loaded into Co-Work sessions manually via the session header protocol. Migration of stable documents to a Claude Project is preserved as a future option if manual context loading becomes a consistent friction point.

---

## Alternatives Considered

- **Claude Projects as primary:** Rejected because Projects lock system context into static uploaded documents, reducing the ability to evolve the system iteratively. Co-Work with GitHub-backed context is more adaptable during early-stage operation. Projects can be layered in later for stable documents once the system has stabilized.

- **Fragmented chats per topic:** Rejected outright. Fragmentation destroys continuity and is the explicit anti-pattern this system is designed to prevent.

---

## Consequences

**Positive:**
- Single coherent operating environment — no context fragmentation across parallel chats or projects
- Context loading is intentional and explicit — forces discipline about what is actually relevant each session
- System documents evolve freely in GitHub without being locked into a Claude Project's static state
- Co-Work file access allows direct integration with the workspace folder

**Negative / accepted tradeoffs:**
- Context must be loaded manually at each session start — no automatic preloading of system documents
- Discipline required to maintain the session header protocol; sessions without proper context loading will have weaker continuity
- If stable documents migrate to a Claude Project later, a migration step is required

---

## Review Trigger

Revisit if: manual context loading becomes a consistent source of friction after 10+ real sessions, or if Claude Projects adds persistent memory features that reduce the tradeoff.

---

## Commit Reference

`adr(001): cowork-as-primary-cockpit`
