# Scoring Model

The referee produces a weighted platform scorecard at the end of every run. Scores are 0-10 with a one-paragraph narrative justification. Scores without narrative are rejected.

## Weighted Categories

| Category | Weight | Owning board(s) |
|---|---|---|
| Security | 20% | Batch 1 |
| Reliability | 20% | Batch 3 (operability), Batch 6 (reliability lens) |
| Maintainability | 15% | Batch 4 |
| Operational Excellence | 15% | Batch 3 |
| Architectural Quality | 15% | Batch 7 (primary), Batch 2 (infra slice), Batch 9 (docs-make-architecture-sustainable slice) |
| Developer Experience | 10% | Batch 6 (primary), Batch 9 (doc-fitness slice) |
| Cost Efficiency | 5% | Batch 8 |

> Weighting is calibrated for internal, non-internet-facing platforms with small SRE teams. For public-facing systems, the referee MAY swap Security ↔ Reliability weights with explicit justification.

## Score Rubric (0-10)

| Range | Meaning | Renewal-conversation framing |
|---|---|---|
| 9-10 | Best-in-class. Resilient under stress. No known systemic risk. | "Industry-leading on this dimension." |
| 7-8 | Solid. Minor issues; trending positive. | "Strong; we monitor known small risks." |
| 5-6 | Acceptable. Material gaps with known remediation. | "Adequate for current scale; investment path mapped." |
| 3-4 | Concerning. Known unaddressed risks. | "Investment required this quarter to avoid degradation." |
| 0-2 | Critical. Active risk to operations or contract obligations. | "Immediate intervention required." |

## Composite Score

```
composite = Σ (category_score × weight)
```

The composite is rounded to one decimal. It is **never** the headline of the executive briefing — the headline is the *trajectory* (Δ vs previous run) plus the highest-risk category. Composite scores in isolation invite vanity metrics.

## Trend Tracking

The risk register CSV captures every finding with severity and category. Over time, a trend file (auto-appended) tracks:

- composite score over time
- category scores over time
- finding counts by severity, by category, by board
- mean time from finding-raised to finding-closed

The trend file lives at `Architecture_Knowledge_Base/quality-reviews/SCORECARD_TREND.csv`. The referee appends one row per run; previous rows are preserved.

CSV header:

```
date,scope,depth,composite,security,reliability,maintainability,opex,architecture,devex,cost,critical,high,medium,low,viability_verdict,operability_gate,sustainability_decisions
```

## Scorecard Anti-Patterns

The scorecard is **not**:

- a leaderboard between teams
- a justification for any specific budget line item
- a substitute for reading findings
- a green/yellow/red dashboard
- the headline of any artifact

It is a longitudinal signal. Trends matter; single-run scores are context.
