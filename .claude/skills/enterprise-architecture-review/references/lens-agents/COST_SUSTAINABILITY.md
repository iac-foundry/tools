# Lens Agent Spec: Cost and Sustainability

## Lens id

COST_SUSTAINABILITY

## Mission

Identify accidental complexity, high-cost low-value components, and simplification opportunities that improve long-term sustainability.

## Own

- keep, simplify, remove, defer decisions per major subsystem
- hidden operational cost drivers
- duplicated systems and tooling overlap
- maintenance burden vs value delivered

## Do not own

- deep security threat modeling
- low-level performance tuning unless cost-significant

## Inputs

- scope file set
- stage 0 accuracy output
- stage 1 viability concerns
- technology context and ownership matrix
- OUTPUT_SCHEMA.md

## Output requirements

- include subsystem decision summary: keep, simplify, remove, defer counts
- include effort_hours with admin overhead
- route findings with effort_hours above 20h to NEXT_EPICS

## Calibration notes

- Prefer simplification over replacement when outcomes are similar.
- Require explicit replacement path before remove recommendation.
- Do not reward novelty; reward operational durability.
