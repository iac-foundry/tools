<!-- Parent: Quality Reviews -->
<!-- Parent: Architecture -->
<!-- Title: Risk Register {DATE} -->
<!-- Label: akb-managed -->

> **NOTICE:** This page is generated from the source repository. Do not edit in Confluence — update the repository instead, or your changes will be overwritten.

# Risk Register — {DATE}

**Date:** {DD Month YYYY}
**Source:** Generated from `enterprise-architecture-review` Stage 3 referee output.
**Trendable form:** [`risk_register-{DATE}.csv`](./risk_register-{DATE}.csv)
**Historical trend:** [`SCORECARD_TREND.csv`](./SCORECARD_TREND.csv)

Severity rubric and finding schema are defined in the skill at:
- `.claude/skills/enterprise-architecture-review/references/severity_rubric.md`
- `.claude/skills/enterprise-architecture-review/references/finding_schema.md`

---

## Summary by Severity

| Severity | Count | New this run | Recurring | Escalated this run |
|---|---|---|---|---|
| CRITICAL | {n} | {n} | {n} | {n} |
| HIGH | {n} | {n} | {n} | {n} |
| MEDIUM | {n} | {n} | {n} | {n} |
| LOW | {n} | {n} | {n} | {n} |

## Summary by Category

| Category | Count | Top finding |
|---|---|---|
| Security | {n} | {EAR-id — title} |
| Infrastructure | {n} | {…} |
| Operability | {n} | {…} |
| Reliability | {n} | {…} |
| Engineering | {n} | {…} |
| Performance | {n} | {…} |
| Developer Experience | {n} | {…} |
| Cost | {n} | {…} |
| Sustainability | {n} | {…} |
| Documentation | {n} | {…} |

---

## Findings

| ID | Severity | Confidence | Category | Scope | Board | Title | Roadmap target | Status | Priority |
|---|---|---|---|---|---|---|---|---|---|
| EAR-{date}-001 | CRITICAL | High | Operability | Platform | Batch 3 | {title} | NEXT_TICKETS | new | P0 |
| EAR-{date}-002 | HIGH | High | Infrastructure | Platform | Batch 2 | {title} | NEXT_TICKETS | recurring (2 runs) | P0 |
| EAR-{date}-003 | HIGH | Medium | Security | Platform | Batch 1 | {title} | NEXT_EPICS | new | P1 |
| … | | | | | | | | | |

---

## Recurring Findings (open more than one run)

| ID | Title | First seen | Runs open | Why still open |
|---|---|---|---|---|
| EAR-{older-id} | {…} | {YYYY-MM-DD} | {n} | {one sentence} |

A recurring finding open across 3+ runs is itself a meta-finding — record it under the Systemic Patterns section of the quality review.

---

## Accepted Risks (roadmap_target == RISK_REGISTER_ONLY)

Findings that are tracked but not currently scheduled for remediation. Each has an explicit rationale and a re-evaluation trigger.

| ID | Title | Severity | Rationale for accepting | Re-evaluation trigger |
|---|---|---|---|---|
| EAR-{id} | {…} | {…} | {…} | {when X happens} |

---

## How This Register is Maintained

- Updated each time the `enterprise-architecture-review` skill runs.
- IDs are stable across runs — a recurring finding keeps its `EAR-YYYY-MM-DD-NNN` id from its original run, and its `status` becomes `recurring`.
- Findings move to `closed` when the remediation ticket completes; closed entries are preserved in the CSV but removed from this markdown for readability.
- The CSV form is the durable record. This markdown is a human-readable view of the same data.
