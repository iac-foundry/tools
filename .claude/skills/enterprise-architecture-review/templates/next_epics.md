<!-- Parent: Infrastructure Automation -->
<!-- Parent: Roadmap -->
<!-- Title: Next Epics -->
<!-- Label: akb-managed -->

> **NOTICE:** This page is generated from the source repository: https://bitbucket.org/iotel/architecture-knowledge-base/src/main/
> Do not edit this page directly in Confluence - update the repository instead, or your changes will be overwritten.

# Next Epics

**Last updated**: {DD Month YYYY}
**Maintained by**: `enterprise-architecture-review` skill

## Preamble

This file is the single-source list of candidate epics and features to pick up when capacity allows. Each entry is a strategic-sized unit (> 20h; will need decomposition into `NEXT_TICKETS.md` tickets before work starts).

Agents and planners must treat each entry as a starting point that requires refinement: provide a short description, desired outcome, acceptance criteria, dependencies, and next steps. Use prioritization factors such as security impact, operational risk, and cross-team dependencies.

Strategic findings from the `enterprise-architecture-review` skill land here when their estimated complexity is `L` or `XL`, or when the referee tags them `roadmap_target: NEXT_EPICS`.

## How this file is updated

- The skill merges new epics in priority order.
- Existing epics are updated in place when scope or status changes.
- Completed epics are preserved (status: ✅ Done) and never deleted.
- Owner and estimate are kept current.

---

## Epics

### {Epic title}

- **Status:** ⬜ Planned | 🟡 In Progress | ✅ Done | ⏸ Deferred
- **Description:** {one paragraph}
- **Desired outcome:** {one paragraph}
- **Acceptance criteria:**
  - {…}
  - {…}
- **Dependencies:** {…}
- **Next steps:** {first sprint's worth of decomposition}
- **Reference:** [{link to related design / standard}](../design/{...}.md)
- **Source finding:** {EAR-id from quality review, if applicable}

---

## Usage notes

- Treat entries here as starting points — create a ticket per epic with clear acceptance criteria and timeline before assigning work.
- Prioritize by security impact and operational risk; prefer small first deliverables (PoC → staging → production).
- Keep `Owner` and `Estimate` fields up to date as tickets are created and refined.
- When an epic decomposes into shippable units, create entries in [`NEXT_TICKETS.md`](./NEXT_TICKETS.md) (each ≤ 20h, including admin overhead) and link back to this epic.

---

*File maintained by the `enterprise-architecture-review` skill. Completed epics are preserved.*
