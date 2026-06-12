# Finding Schema

Every finding emitted by a board or the referee MUST conform to this schema. The CSV risk register is generated directly from this shape, so the field order matters.

## Required Fields

| # | Field | Type | Notes |
|---|---|---|---|
| 1 | `id` | string | `EAR-YYYY-MM-DD-NNN`. Allocated by the referee at consolidation time. |
| 2 | `title` | string (≤ 80 chars) | Imperative, evidence-anchored. e.g. `Reconcile ubuntu_vm/vsphere module version ranges`. |
| 3 | `severity` | enum | `CRITICAL` / `HIGH` / `MEDIUM` / `LOW` per `severity_rubric.md`. |
| 4 | `confidence` | enum | `High` / `Medium` / `Low`. |
| 5 | `scope` | enum | `Platform` / `Domain` / `Repository` / `Module`. |
| 6 | `category` | enum | `Security` / `Infrastructure` / `Operability` / `Reliability` / `Engineering` / `Performance` / `DeveloperExperience` / `Cost` / `Sustainability` / `Documentation`. |
| 7 | `board` | string | Which board raised it (`Batch 1` … `Batch 8`, `Referee`, `Stage 0`, `Stage 1`). |
| 8 | `evidence` | string | Specific file paths, line refs, symbols, commands, or runtime observations. |
| 9 | `problem_statement` | string | What is wrong, expressed as a single direct sentence. |
| 10 | `operational_impact` | string | Runtime / incident / operability implications. |
| 11 | `business_impact` | string | Delivery, contract, or risk implications in plain language. |
| 12 | `recommended_action` | string | Concrete, shippable remediation. Should map to a ticket or epic. |
| 13 | `strategic_value` | string | Why this matters beyond the immediate fix. |
| 14 | `estimated_complexity` | enum | `XS` (<½d) / `S` (1d) / `M` (2d) / `L` (3-5d, epic) / `XL` (>1 sprint, requires breakdown). |
| 15 | `tradeoffs` | string | Risks introduced or costs incurred by the remediation. |
| 16 | `intent_class` | enum | `intentional-design` / `historical-accident` / `temporary-workaround` / `necessary-complexity` / `accidental-complexity`. |
| 17 | `cross_repo_refs` | string \| empty | Other repos exhibiting the same pattern. Empty if isolated. |
| 18 | `roadmap_target` | enum | `NEXT_TICKETS` / `NEXT_EPICS` / `RISK_REGISTER_ONLY`. |
| 19 | `priority_band` | enum | `P0` / `P1` / `P2` / `P3` / `P4`. Assigned by referee. |
| 20 | `status` | enum | `new` / `recurring` / `escalated` / `superseded`. Default `new`. |

## CSV Header

The risk register CSV uses the following exact header line:

```
id,title,severity,confidence,scope,category,board,evidence,problem_statement,operational_impact,business_impact,recommended_action,strategic_value,estimated_complexity,tradeoffs,intent_class,cross_repo_refs,roadmap_target,priority_band,status
```

All free-text fields must be quoted and embedded newlines escaped as `\n`.

## Markdown Findings Block (per finding in the quality-review document)

```markdown
### EAR-2026-05-16-001 — Reconcile ubuntu_vm/vsphere module version ranges

- **Severity:** HIGH   **Confidence:** High   **Priority:** P0
- **Scope:** Platform   **Category:** Infrastructure   **Board:** Batch 2
- **Evidence:** `docker_servers/main.tf` pins `>=1.0.0,<2.0.0`; `infra-sec-01/main.tf` pins `>=0.1.5,<0.2.0`. Ranges do not overlap.
- **Problem:** Two production workspaces cannot consume the same release of the shared module.
- **Operational impact:** A future bugfix release of `ubuntu_vm/vsphere` cannot be adopted by both workspaces simultaneously; silent compatibility break during rebuild.
- **Business impact:** Platform rebuild non-determinism; contract-relevant uptime claim weakened.
- **Recommended action:** Choose a single overlapping range; record canonical version in `decisions/`; align both workspaces.
- **Strategic value:** Restores module portability; preconditions for cross-workspace upgrade campaigns.
- **Complexity:** S (1d). **Tradeoffs:** One workspace pays a small upgrade cost.
- **Intent class:** historical-accident   **Cross-repo refs:** docker_servers, infra-sec-01
- **Roadmap target:** NEXT_TICKETS   **Status:** new
```

## How findings become roadmap entries

| Finding traits | Goes to |
|---|---|
| `estimated_complexity ∈ {XS, S, M}` AND `roadmap_target == NEXT_TICKETS` | New row in `NEXT_TICKETS.md` |
| `estimated_complexity ∈ {L, XL}` OR `roadmap_target == NEXT_EPICS` | New entry in `NEXT_EPICS.md` |
| `roadmap_target == RISK_REGISTER_ONLY` | Risk register only (e.g. accepted risks, watch items) |

A finding never produces both a ticket and an epic. If decomposition is required, the referee writes the parent as an epic in `NEXT_EPICS.md` and the first decomposed unit as the ticket in `NEXT_TICKETS.md`.
