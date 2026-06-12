<!-- Parent: Quality Reviews -->
<!-- Parent: Architecture -->
<!-- Title: Workspace Architecture Review {DATE} -->
<!-- Label: akb-managed -->

> **NOTICE:** This page is generated from the source repository. Do not edit in Confluence — update the repository instead, or your changes will be overwritten.

# Workspace Architecture Review — {DATE}

**Status:** Active
**Date:** {DD Month YYYY}
**Scope:** {workspace | domain:<name> | project:<name>}
**Depth:** {deep | standard | fast}
**Review mode:** {code-only | code-plus-runtime}
**Owner:** Platform Engineering
**Reviewers:** {names / boards}
**Related:**
- [Executive Briefing](./EXECUTIVE_BRIEFING-{DATE}.md)
- [Risk Register (md)](./RISK_REGISTER-{DATE}.md)
- [Risk Register (csv)](./risk_register-{DATE}.csv)
- [NEXT_TICKETS](../roadmap/NEXT_TICKETS.md) · [NEXT_EPICS](../roadmap/NEXT_EPICS.md)

---

## Executive Summary

{Two to four short paragraphs. Each paragraph delivers one verdict:}

- **Architectural quality verdict:** {one paragraph}
- **Operational sustainability verdict:** {one paragraph}
- **Rebuild confidence verdict:** {one paragraph}
- **Complexity assessment:** {one paragraph}

**Single highest-leverage observation:** {one sentence}

---

## Stage Gates

| Stage | Verdict | Notes |
|---|---|---|
| Stage 0 — Accuracy Gate | {PASS / PASS-WITH-NOTES / FAIL} | {n docs reviewed, n PASS, n FAIL} |
| Stage 1 — Viability Gate | {PROCEED / PROCEED-WITH-CONCERNS / STOP} | {concerns listed if any} |
| Batch 3 — Operability Gate | {PASS / FAIL} | {key drivers} |
| Batch 8 — Sustainability Decision Gate | {KEEP n / SIMPLIFY n / REMOVE n / DEFER n} | over-engineering: {APPROPRIATELY SIZED / UNDER / OVER} |

---

## Scorecard

| Category | Weight | Score | Trend vs prev | Narrative |
|---|---|---|---|---|
| Security | 20% | {0-10} | {↑ / → / ↓} | {one sentence} |
| Reliability | 20% | {0-10} | {↑ / → / ↓} | {one sentence} |
| Maintainability | 15% | {0-10} | {↑ / → / ↓} | {one sentence} |
| Operational Excellence | 15% | {0-10} | {↑ / → / ↓} | {one sentence} |
| Architectural Quality | 15% | {0-10} | {↑ / → / ↓} | {one sentence} |
| Developer Experience | 10% | {0-10} | {↑ / → / ↓} | {one sentence} |
| Cost Efficiency | 5% | {0-10} | {↑ / → / ↓} | {one sentence} |
| **Composite** | — | **{N.N}** | {↑ / → / ↓} | — |

Trend file: [`SCORECARD_TREND.csv`](./SCORECARD_TREND.csv)

---

## Top-N Priority Roadmap

Strict ordering. Each item links to its risk register entry and target roadmap file.

| # | Item | Severity | Impact | Urgency | Operational value | Simplification | Risk reduction | Roadmap target |
|---|---|---|---|---|---|---|---|---|
| 1 | {title} | {C/H/M/L} | {one phrase} | {Immediate/Near-term/Medium} | {High/Med/Low} | {High/Med/Low} | {High/Med/Low} | {NEXT_TICKETS / NEXT_EPICS} |
| … |

---

## Systemic Architectural Patterns

{One subsection per named pattern. A pattern is worth naming when it appears in 3+ findings across 2+ boards.}

### 1. {Pattern name}

{Two to four sentences. Cite the contributing findings by id. State the operational consequence and the remediation arc.}

### 2. {Pattern name}

{…}

---

## High-Leverage Simplifications

{Three to five changes with disproportionate operational benefit relative to effort. These are the ones to talk about in strategy meetings.}

### A. {Simplification title}

{Two to three sentences. Effort estimate. Expected delta in operational burden.}

### B. {…}

---

## Early-Warning Risks

{Architectural directions that, if unaddressed, become operational failures within 6-18 months. Owned by Batch 7.}

1. **{Risk title}** *(item #N from roadmap)* — {trigger event; consequence; time horizon}
2. **{…}**

---

## Findings (Full Detail)

{One subsection per finding, in priority band order. Format per `references/finding_schema.md`.}

### {finding-id} — {title}

- **Severity:** {C/H/M/L}   **Confidence:** {High/Med/Low}   **Priority:** {P0-P4}
- **Scope:** {Platform/Domain/Repository/Module}   **Category:** {…}   **Board:** {Batch N / Referee / Stage 0}
- **Evidence:** {file:line, symbols, commands, observations}
- **Problem:** {one direct sentence}
- **Operational impact:** {…}
- **Business impact:** {…}
- **Recommended action:** {concrete, shippable}
- **Strategic value:** {…}
- **Complexity:** {XS/S/M/L/XL}   **Tradeoffs:** {…}
- **Intent class:** {intentional-design / historical-accident / temporary-workaround / necessary-complexity / accidental-complexity}
- **Cross-repo refs:** {…}
- **Roadmap target:** {NEXT_TICKETS / NEXT_EPICS / RISK_REGISTER_ONLY}   **Status:** {new / recurring / escalated / superseded}

{Repeat per finding.}

---

## Documentation Drift Findings (from Stage 0)

| Document | Verdict | Critical/High drift items |
|---|---|---|
| {path} | {PASS/PASS-WITH-NOTES/FAIL} | {short list or "none"} |

---

## Final Verdict

| Question | Verdict | Justification |
|---|---|---|
| Is the platform understandable? | {yes/no} | {one sentence} |
| Is ownership clear? | {yes/no} | {one sentence} |
| Is the architecture operationally sustainable? | {yes/no} | {one sentence} |
| Is it over-engineered? | {yes/no} | {one sentence} |
| Can it be rebuilt reliably? | {yes/no} | {one sentence} |
| Would new engineers understand it within reasonable time? | {yes/no} | {one sentence} |

---

## Board Disagreements (recorded)

{Empty section if none. Otherwise list each disagreement: the boards involved, the finding, the chosen path, the rationale.}

---

## Method

- **Stages:** {Stage 0 accuracy + Stage 1 viability + 8 specialist boards (parallel) + referee consolidation}
- **Skill:** `enterprise-architecture-review` (project skill, version-controlled in `.claude/skills/`)
- **Procedure:** [`procedures/ARCHITECTURE_REVIEWS.md`](../procedures/ARCHITECTURE_REVIEWS.md)
- **Repos surveyed:** {list}

---

*Generated by the `enterprise-architecture-review` skill on {DATE}. Source of truth: this repository.*
