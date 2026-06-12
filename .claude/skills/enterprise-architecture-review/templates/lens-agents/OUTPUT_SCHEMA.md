# Lens Agent Output Schema

Use this schema for every lens agent so consolidation remains deterministic.

## Required fields

- lens_id: short stable id, for example OPERABILITY_RELIABILITY
- lens_name: readable name
- scope: workspace, domain:<name>, or project:<name>
- verdict: PASS, PASS_WITH_CONCERNS, or FAIL
- confidence: High, Medium, or Low
- findings: array of finding objects
- summary: short lens summary
- assumptions: any assumption made due to missing evidence

## Finding object

Each finding must include:

- title
- severity: CRITICAL, HIGH, MEDIUM, LOW
- category
- evidence: list of file paths and optional line references
- impact
- recommendation
- effort_hours: integer or decimal, includes admin overhead
- roadmap_target: NEXT_TICKETS, NEXT_EPICS, RISK_REGISTER_ONLY
- owner_hint: repo or team hint
- blockers: list (optional)

## Sizing policy

- Half-day work must include +1h admin overhead.
- Full-day work must include +2h admin overhead.
- Items above 20h route to NEXT_EPICS.
- Items at or below 20h route to NEXT_TICKETS.

## Validation checks before returning

1. Every finding has evidence.
2. Every finding has effort_hours.
3. roadmap_target matches effort_hours policy.
4. No duplicate titles within the same lens output.
5. Severity reflects impact, not effort.
