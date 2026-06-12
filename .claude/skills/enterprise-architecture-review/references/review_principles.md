# Review Principles

These principles are loaded into every board and the referee. They are the contract for what a finding must look like.

## 1. No Sacred Cows

No technology, framework, abstraction, process, or historical design decision is exempt.

A component is open for challenge when any of the following is true:

- it no longer aligns with current best practice
- a simpler alternative exists
- complexity exceeds delivered value
- maintenance burden is disproportionate
- risk is disproportionate
- operational fragility is present

## 2. System-Level Thinking

Repos are not reviewed in isolation. The platform is evaluated as:

- a connected operational system
- a delivery ecosystem
- a security boundary
- a runtime topology
- a dependency graph
- a socio-technical environment

Required cross-repo lenses:

- duplication of logic / infrastructure / CI-CD patterns / security models
- conflicting patterns and divergent standards
- abstraction or platformization opportunities
- organizational scaling bottlenecks

## 3. Adversarial Review

Each board actively challenges assumptions. Specifically look for:

- fragility, hidden failure domains, implicit coupling
- undefined ownership, operational anti-patterns
- over-engineering and under-engineering
- security weaknesses, reliability gaps, poor observability
- scalability limits, human-dependency risks, knowledge silos

A board that produces only "looks fine" output has failed.

## 4. Evidence-Based Findings

Every finding must include:

- specific file/path/symbol references
- the observed behavior or pattern
- runtime implications (if any)
- cross-repository references (when systemic)

"This feels off" without evidence is noise. Drop it.

## 5. Explainability

Every finding includes:

- evidence
- context
- risk description
- why the current approach is problematic
- recommended alternative
- tradeoffs
- estimated impact
- confidence level (high / medium / low)

AI-generated findings must be explainable and traceable.

## 6. Distinguish Intent from Accident

A finding must classify what it is looking at as one of:

- intentional design
- historical accident
- temporary workaround
- necessary complexity
- accidental complexity

The remediation differs by class. Do not treat accidental complexity and necessary complexity the same way.

## 7. The 2am Test (Mandatory)

A tired, unfamiliar engineer must be able to debug and recover the system at 2am. If unclear, that is a finding — not a note.

Examples of 2am-test failures:

- the recovery path requires reading code, not config
- the runbook references a flow that no longer exists
- the system depends on tribal knowledge held by one person
- diagnostic state lives only inside a TF state file or an ephemeral pipeline run

## 8. Failure Mode Analysis (Mandatory)

For each domain area, document:

- first point of failure
- cascade risks
- silent failure scenarios
- recovery path complexity

## 9. Simplicity Check (Mandatory)

For each domain area, surface:

- unnecessary complexity
- duplicated intent
- over-abstraction relative to problem size
- abstractions that are not paying for themselves

## 10. Operational Simplicity Beats Theoretical Correctness

For this platform (small SRE team, high uptime requirements, non-internet-facing):

- fewer moving parts > elegant abstractions
- explicit > implicit
- boring > novel
- recoverable > optimized
- composable > frameworked

A finding that recommends adding complexity must justify the operational cost.

## 11. Findings Carry Actions

A finding without a remediation roadmap entry is incomplete. The referee converts every CRITICAL/HIGH finding into either:

- a sprint-sized ticket in `NEXT_TICKETS.md` (≤ 20h, including admin overhead), or
- a strategic epic in `NEXT_EPICS.md` (larger than 20h, requiring breakdown)

Never both.

## 12. Documentation Must Match Reality

Drift between code and AKB documents (`design/`, `standards/`, `runbooks/`, `decisions/`) is itself a finding. The Stage 0 accuracy gate quantifies it. A platform that ships well but documents poorly will fail onboarding and recovery.

## 13. Standards Must Be Enforceable

A standard that linting / CI cannot validate is aspirational, not real.

When a finding cites a standards violation, the referee asks: "Why didn't lint catch this?" If the answer is "we have no lint for it," the missing lint becomes its own finding.

## 14. Anti-Goals

This review must not become:

- a linting exercise
- a compliance-only process
- a bureaucracy generator
- a tool-driven checkbox system
- a style-enforcement mechanism
- a blocker to engineering progress
- an approval rubber stamp

Findings that drift toward any of the above are dropped.
