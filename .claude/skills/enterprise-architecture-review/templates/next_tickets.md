<!-- Parent: Infrastructure Automation -->
<!-- Parent: Roadmap -->
<!-- Title: Next Tickets -->
<!-- Label: akb-managed -->

> **NOTICE:** This page is generated from the source repository: https://bitbucket.org/iotel/architecture-knowledge-base/src/main/
> Do not edit this page directly in Confluence - update the repository instead, or your changes will be overwritten.

# Next Tickets

Sprint-planning backlog and roadmap source of truth for platform execution.

> **Maintenance**: This file is maintained by the `enterprise-architecture-review` skill. The skill regenerates ticket bands, preserves completed rows, and normalizes state labels. Roadmap diagrams are generated in [`ROADMAP_GRAPH.md`](./ROADMAP_GRAPH.md) as multiple Mermaid graphs.

> **Roadmap reconciliation**: Before each write, the skill verifies active ticket status against current code/docs evidence. Tickets found complete are marked `✅ Done` even when legacy state fields were not manually updated. Open epics are re-scored each run and important epics are implicitly decomposed into ticket-sized entries unless manual-only epic inclusion is explicitly requested.

> **Jira alignment**: Jira IO board and backlog are mandatory prioritization inputs. Jira issues in Backlog and To Do are considered for prioritization. Jira To Do items are treated as in-flight work and mapped to roadmap rows using Jira keys (for example `IO-123`) to prevent duplicate competing entries.

This list is intentionally short-ticket and execution-first.

- Every ticket must be isolated and independently shippable.
- Every ticket must target ≤ 20h effort (hours include mandatory admin overhead).
- Complete strictly in priority order unless a ticket is explicitly blocked.

**Last updated**: {DD Month YYYY}
**Planning horizon**: {short description of current cycle}
**Primary goal**: {one sentence}
**Companion document**: [`../quality-reviews/workspace_architecture_review-{DATE}.md`](../quality-reviews/workspace_architecture_review-{DATE}.md) — strategic architecture review that this roadmap operationalises.

## Ticket sizing rules

1. If a ticket is estimated > 20h total, split it before sprint commitment (escalate larger items to [`NEXT_EPICS.md`](./NEXT_EPICS.md)).
2. Apply admin overhead policy in sizing: half-day work adds `+1h`; full-day work adds `+2h`.
3. Each ticket has one clear definition of done.
4. Tickets must not mix platform hardening and application migration in the same item.
5. Prefer fail-fast verification tasks early so defects surface before cutover.

## State labels

| Label | Meaning |
|---|---|
| ✅ Done | Completed (preserved across runs) |
| 🟡 In Progress | Active in current sprint |
| ⬜ Planned | Not yet started |
| ⛔ Blocked | Dependency missing |
| ⏸ Deferred | Intentionally postponed (with reason) |

---

## Priority order

### P0 — {Band theme: must complete first}

| ID | Ticket | Repo | Effort (h, incl admin) | State | Jira | Definition of done |
|---|---|---|---|---|---|---|
| P0-XX | {title} | {repos} | {hours}h | ⬜ Planned | - | {DoD} |

### P1 — {Band theme}

| ID | Ticket | Repo | Effort (h, incl admin) | State | Jira | Definition of done |
|---|---|---|---|---|---|---|

### P2 — {Band theme}

| ID | Ticket | Repo | Effort (h, incl admin) | State | Jira | Definition of done |
|---|---|---|---|---|---|---|

### P3 — {Band theme}

| ID | Ticket | Repo | Effort (h, incl admin) | State | Jira | Definition of done |
|---|---|---|---|---|---|---|

### P4 — {Band theme: nice-to-have / future}

| ID | Ticket | Repo | Effort (h, incl admin) | State | Jira | Definition of done |
|---|---|---|---|---|---|---|

---

## Roadmap graphs

See [`ROADMAP_GRAPH.md`](./ROADMAP_GRAPH.md) for dependency visualization and per-epic decomposition graphs.

---

*File maintained by the `enterprise-architecture-review` skill. Completed tickets are preserved. State labels rendered as visual labels (no checkboxes) for Confluence compatibility.*
