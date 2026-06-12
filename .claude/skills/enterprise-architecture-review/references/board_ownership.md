# Board Ownership Matrix

Each board has explicit ownership and explicit non-ownership. The non-ownership lines exist to prevent finding duplication and to force each board to stay in its lane. If a finding feels like it belongs to multiple boards, the referee assigns the canonical owner per this matrix and escalates severity once.

## Batch 1 — Security & Trust

**Owns:**
- identity (Authentik, SSSD, LDAP)
- secrets management (Vault, AppRole, Vault Agent, token files)
- trust boundaries and execution privilege models
- attack surface analysis (SSH, OIDC, API endpoints)
- supply-chain risk (collection sources, image provenance)
- secret distribution patterns (cloud-init, host-side Vault, withVault helpers)
- IAM models, RBAC, group claims, lateral movement paths
- TLS / CA trust and certificate handling

**Does not own:**
- compliance formalism (paperwork-only checklists)
- infrastructure topology (Batch 2)
- pipeline orchestration (Batch 3)
- code quality patterns (Batch 4)

## Batch 2 — Infrastructure & Rebuildability

**Owns:**
- Terraform topology, module structure, backend / state design
- environment isolation, workspace boundaries
- inventory architecture, host identity registry
- orchestration sequencing for IaC
- idempotency and drift prevention
- Packer image identity and versioning
- dependency modeling (TF ↔ Ansible boundary, module versions)
- rebuild determinism
- **data lifecycle and recovery** (cross-cutting, lives here)
- **FinOps and capacity risk** (cross-cutting, lives here)

**Does not own:**
- application-layer security controls (Batch 1)
- pipeline orchestration logic (Batch 3)
- code patterns inside roles (Batch 4)
- runtime performance characteristics (Batch 5)

## Batch 3 — Operability & Reliability  *(HARD GATE)*

**Owns:**
- pipeline behavior, rollback capability, deploy safety
- observability — metrics, logs, traces, dashboards, alerting
- failure isolation and cascade analysis
- debugging ergonomics, runbook realism, on-call experience
- disaster recovery posture and recovery determinism
- the 2am test as platform-wide assertion
- **supply chain integrity** (cross-cutting, lives here — artifacts in transit through pipelines)
- **operational human factors** (cross-cutting, lives here)

**Hard gate output (required):**
A single explicit verdict in the board report:

```
OPERABILITY VERDICT: PASS | FAIL
```

A FAIL verdict is automatically a CRITICAL platform-level finding and locks at CRITICAL through referee consolidation.

**Does not own:**
- code structure inside roles (Batch 4)
- security controls (Batch 1)
- IaC module design (Batch 2)
- cost / sustainability decisions (Batch 8)

## Batch 4 — Engineering & Quality

**Owns:**
- code structure and patterns (Ansible role design, Terraform module design, shared lib code)
- abstraction quality and consistency
- duplication detection (code-level, not infrastructure-level — that's Batch 2)
- naming clarity and convention adherence
- variable sprawl, defaults discipline
- test quality and meaningful assertions
- design-implementation fidelity (does the code match the AKB design doc?)

**Does not own:**
- infrastructure decisions (Batch 2)
- security posture (Batch 1)
- deployment orchestration (Batch 3)
- developer ergonomics around tooling (Batch 6)

## Batch 5 — Performance & Scalability

**Owns:**
- runtime efficiency of operational paths (cold-start, rebuild time, pipeline duration)
- resource utilization patterns (CPU, memory, disk, network)
- caching strategies
- query / API patterns that hit external systems (Vault, Authentik, vSphere)
- concurrency and parallelism in pipelines and orchestration
- scaling behavior under load

**Does not own:**
- application-internal performance (out of platform scope unless platform-impacting)
- IaC topology efficiency (Batch 2)

## Batch 6 — Developer Experience & Cognitive Load

**Owns:**
- onboarding friction and time-to-productivity
- documentation discoverability (does the right doc exist; can you find it?)
- tooling ergonomics, local-dev workflow
- CI/CD usability for engineers (not pipeline correctness — that's Batch 3)
- error message quality
- consistency of patterns across repos (engineer churn risk)

**Does not own:**
- documentation accuracy (Stage 0 accuracy gate owns drift; Batch 6 owns whether docs are *findable* and *usable*)
- pipeline correctness (Batch 3)
- test quality (Batch 4)

## Batch 7 — Platform Strategy & Technology Evolution

**Owns:**
- architectural coherence at the platform level
- technology-choice currency (is the chosen stack still appropriate?)
- evolution pathways (where is the platform heading; is it sustainable?)
- platformization opportunities (what should become a shared capability?)
- organizational scaling implications
- alignment to industry direction (when material, not when fashionable)

**Does not own:**
- specific module or role decisions (Batch 4)
- operational current-state assessment (Batch 3)

## Batch 8 — Cost, Efficiency & Sustainability  *(DECISION GATE)*

**Owns:**
- operational overhead vs delivered value
- infrastructure utilization vs provisioning
- engineering effort wasted on accidental complexity
- maintenance burden disproportionality
- **keep / simplify / remove / defer** decisions per major subsystem
- over-engineering verdict

**Decision gate output (required):**
For each major subsystem, exactly one of:

```
KEEP      — justify operationally
SIMPLIFY  — name the simplification, target effort
REMOVE    — name the replacement or accepted gap
DEFER     — name the trigger condition that would re-open the decision
```

No general findings. Decisions only.

**Does not own:**
- new feature recommendations (out of scope by design)
- cost as a function of one vendor's pricing model (that's a procurement conversation)

## Batch 9 — Documentation Quality  *(emits ready-to-apply edits)*

**Owns:**
- writing-quality and Diataxis type fitness of `design/`, `standards/`, `runbooks/`, `decisions/`, `procedures/`
- per-type quality checklists (How-to, Tutorial, Reference, Explanation, Standard, ADR)
- fitness-for-reader verdict per document
- defect detection: type mixing, sprawl, stubs published as complete, dead internal/external links, broken structure, inconsistent depth
- emission of *ready-to-apply edits* in a structured block the main thread consumes during Step 7b

**Special output (required):**
For every finding with a straightforward textual fix, the board emits a ready-to-apply edit block (`replace` / `insert_before` / `insert_after` / `delete` / `manual`) anchored on a unique substring. The Step 7b prompt asks the user whether to apply the edits to source documents.

**Does not own:**
- documentation **accuracy** (doc-vs-code drift) → Stage 0
- documentation **discoverability** (can a reader find it?) → Batch 6
- repo READMEs → Batch 6
- roadmap files → structurally constrained by the skill itself
- code comments and inline documentation → Batch 4
- meta-documentation in `.claude/skills/` and `templates/` → out of scope

## Cross-Cutting Concern Ownership Summary

| Concern | Owner |
|---|---|
| Identity and access operations | Batch 1 |
| Data lifecycle and recovery | Batch 2 |
| FinOps and capacity risk | Batch 2 |
| Supply chain integrity | Batch 3 |
| Operational human factors | Batch 3 |
| Documentation accuracy | Stage 0 |
| Documentation discoverability | Batch 6 |
| Documentation writing quality and type fitness | Batch 9 |
