# Severity Rubric

All boards and the referee use these definitions consistently.

Severity is **not security-exclusive**. A complexity issue that causes operator error at 2am is CRITICAL.

| Severity | Definition | Representative examples |
|---|---|---|
| **CRITICAL** | Will cause outage, data loss, unrecoverable state, or undebuggability at 2am. Must be addressed immediately. | Missing auth on a privileged path. No backup/restore path. Single point of failure with no failover. Recovery requires tribal knowledge. Bootstrap circularity that blocks platform rebuild. Standards reference a component that does not exist. |
| **HIGH** | Likely to fail under stress or incident conditions; substantially raises operational risk. | Cascade risk with no circuit breaker. Unclear recovery runbook. Retry storm potential. Implicit cross-tool coupling. Vault/secret distribution coupled to provisioning. Inconsistent version pins across workspaces. |
| **MEDIUM** | Maintainability or reliability degradation that compounds over time. | Inconsistent error handling. Missing failure-path tests. Config drift risk. Duplication of truth across 2-3 places. Documentation rot. Vendored copy lagging upstream. |
| **LOW** | Improvement opportunity, not immediate risk. | Naming inconsistency. Minor simplification. Cosmetic UI polish. Documentation gap that does not affect operability. |

## Severity Calibration Rules

- A finding that surfaces in **multiple boards** is escalated by one severity level (max CRITICAL). The referee records the escalation in the consolidated entry.
- A finding marked CRITICAL by **Batch 3 (Operability)** is locked at CRITICAL regardless of other boards' assessment. Operability has hard-gate authority.
- A finding with **HIGH confidence** and **MEDIUM severity** ranks above a finding with LOW confidence and HIGH severity when prioritizing. Confidence is recorded separately (see `finding_schema.md`).

## Confidence Rubric

| Confidence | Meaning |
|---|---|
| **High** | Direct evidence in code/config; reproducible; verifiable in a fresh clone. |
| **Medium** | Strong indirect evidence; pattern across multiple files; behavior implied by config but not directly observed. |
| **Low** | Inferred from one signal; no direct reproduction; further investigation needed. |

Low-confidence findings are still reported but never become CRITICAL until verified.

## What Severity is Not

- Severity is not a popularity contest. The referee does not negotiate severity to fit a sprint capacity.
- Severity is not a function of remediation effort. A trivially-fixable issue can still be CRITICAL.
- Severity is not a function of how long the issue has been present. Old does not mean accepted.
