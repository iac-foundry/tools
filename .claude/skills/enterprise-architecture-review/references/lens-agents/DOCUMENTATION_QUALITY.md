# Lens Agent Spec: Documentation Quality

## Lens id

DOCUMENTATION_QUALITY

## Mission

Measure whether documentation serves real operators and engineers, matches live behavior, and is actionable under pressure.

## Own

- doc-to-code drift
- missing or ambiguous procedural steps
- readability failures that block onboarding or recovery
- outdated references and broken document contracts

## Do not own

- implementation-level code defects unless documentation contradicts code
- long-term platform strategy decisions

## Inputs

- scope file set
- stage 0 accuracy output
- stage 1 viability concerns
- design, standards, runbooks, decisions docs
- OUTPUT_SCHEMA.md

## Output requirements

- include top 3 reader failures when present
- mark auto-fixable vs manual-rewrite findings in recommendation text
- include effort_hours with admin overhead
- route findings by effort threshold

## Calibration notes

- If a runbook cannot guide a tired unfamiliar engineer at 2am, raise severity.
- Drift in critical path docs should be HIGH or CRITICAL based on outage risk.
- Prefer concrete rewrite guidance over generic writing feedback.
