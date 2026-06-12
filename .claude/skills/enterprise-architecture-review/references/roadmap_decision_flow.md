# Roadmap Decision Flow

This diagram defines which inputs are considered, how they are filtered, and the order used when creating or updating roadmap artifacts.

## Stages

- **Stage 0 — Accuracy Gate:** detect doc-to-code drift early and elevate critical mismatches.
- **Stage 1 — Viability Gate:** decide whether to stop, proceed, or proceed with explicit concerns.
- **Stage 2 — Specialist Boards:** produce evidence-backed findings from parallel review lenses.
- **Stage 3 — Referee Consolidation:** deduplicate, score, and produce an ordered action set.
- **Stage 3b — Reality Reconciliation:** verify tickets, epics, and roadmap planning docs against current code/docs before roadmap write.
- **Stage 3c — Jira IO Alignment:** ingest IO board/backlog and align roadmap priorities and in-flight status.
- **Stage 4 — Roadmap Routing:** map actions to tickets or epics using effort and policy rules.
- **Stage 4b — Doc Fix Application:** optionally apply approved doc fixes and close resolved items.

```mermaid
graph TD
    A[Start Scope Resolved] --> B[Stage 0 Accuracy Gate]
    B --> C[Doc Drift Check]
    C --> C1[Raise Severity If Critical]
    C1 --> D[Stage 1 Viability Gate]

    D --> E[Viability Decision]
    E --> E1[Stop Path]
    E --> F[Proceed Path To Stage 2]

    F --> F1[Batch Outputs And Findings]
    F1 --> G[Stage 3 Referee Consolidation]

    G --> G1[Deduplicate Findings]
    G1 --> G2[Apply Weighted Scoring]
    G2 --> G3[Generate Ordered Action List]

    G3 --> R[Stage 3b Reality Reconciliation]
    R --> R1[Validate Active Tickets Against Code]
    R --> R2[Auto Close Stale Done Tickets]
    R --> R3[Re Score Open Epics By Importance]
    R --> R4[Auto Promote Important Epics To Ticket Breakdown]
    R --> R5[Ingest Roadmap Planning Docs]

    R5 --> JIRA[Stage 3c Jira IO Alignment]
    JIRA --> JIRA1[Read IO Board And Backlog]
    JIRA --> JIRA2[Treat Jira To Do As In Flight]
    JIRA --> JIRA3[Map IO Keys And Remove Duplicates]

    JIRA3 --> H[Estimate Effort In Hours]
    H --> H1[Check Over 20h Threshold]
    H1 --> I[Route To Next Epics]
    H1 --> J[Route To Next Tickets]

    J --> J2[Preserve Done Rows]
    J --> J3[Update In Progress Rows]
    J --> J4[Assign Priority Band]
    J --> J5[Enforce Ticket Cap]

    I --> I1[Merge Epic In Priority Order]
    I1 --> I2[Preserve Completed Epics]

    J2 --> K[Regenerate Roadmap Graph File]
    J3 --> K
    J4 --> K
    J5 --> K
    I2 --> K

    K --> L[Write Next Epics Next Tickets And Roadmap Graph]
    L --> M[Batch 9 Doc Fix Decision]
    M --> N[Apply Doc Fixes Path]
    M --> O[Keep Doc Findings Path]
    N --> P[Final Outputs Complete]
    O --> P
```

## Ordered Consideration Checklist

1. Scope and context resolution.
2. Accuracy and doc-drift signal.
3. Viability decision (STOP/PROCEED).
4. Specialist board findings and hard/decision gates.
5. Referee dedupe, scoring, and ordered action list.
6. Reality reconciliation: ticket validity and stale closure correction.
7. Roadmap planning document ingestion from `roadmap/*.md`.
8. Epic importance review and implicit promotion to ticket breakdown.
9. Jira IO board/backlog alignment and in-flight duplicate suppression.
10. Effort normalization in hours, including admin overhead.
11. Routing: <= 20h to tickets, > 20h to epics.
12. Update rules enforcement (preserve done, update in-place, cap handling).
13. Dependency graph regeneration.
14. Optional doc-fix application and roadmap cleanup.
