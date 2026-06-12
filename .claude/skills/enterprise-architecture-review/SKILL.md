---
name: enterprise-architecture-review
description: "Flagship multi-faceted architecture review for the IOTel platform. Runs an adversarial, system-level review across 9 specialist boards (Security, Infrastructure, Operability, Engineering, Performance, Developer Experience, Platform Strategy, Cost/Sustainability, Documentation Quality), gated by accuracy and viability checks, consolidated by a referee. Produces a dated quality-review document, executive briefing, risk register (CSV + markdown), updated NEXT_EPICS.md, updated NEXT_TICKETS.md, and an updated ROADMAP_GRAPH.md with regenerated Mermaid roadmap graphs. Optionally applies documentation fixes directly to source documents. Replaces the older architecture-review, workspace-review, akb-accuracy-gate, update-akb, and code-review skills for IOTel work."
argument-hint: "[--workspace | --domain <name> | <project-path> | <repo-name>] [--deep|--standard|--fast] [--no-accuracy-gate] [--full-review|--roadmap-graph-only]"
disable-model-invocation: true
---

# Enterprise Architecture Review

This is the canonical, opinionated architecture review for the IOTel platform.

It is intentionally adversarial, system-level, and evidence-driven. It produces artifacts used directly in:

- quarterly architecture strategy meetings
- sprint planning and roadmap refinement
- contract renewal conversations (executive briefing)
- risk register tracking over time

This skill **supersedes** the following older skills for any IOTel work:

- `architecture-review` (user-level)
- `workspace-architecture-review` / `workspace-review` (user-level)
- `akb-accuracy-gate` (folded in as Stage 0)
- `update-akb` (folded in as part of Stage 0 documentation reconciliation)
- `code-review` (folded into Batch 4 — Engineering & Quality)

If you are reviewing the IOTel platform, run **this** skill. Do not run the older ones in parallel.

## Mission

> Treat the entire workspace as one operational system. Challenge assumptions. Remove sunk-cost bias. Produce fewer, sharper, evidence-backed findings. Improve operability, rebuildability, security, maintainability, and delivery velocity. Drive continuous architectural evolution.

See `procedures/ARCHITECTURE_REVIEWS.md` for the broader program framing, principles, and governance.

## Usage

```bash
# Full workspace review (default, deep)
/enterprise-architecture-review
/enterprise-architecture-review --workspace

# Domain review — a named group of related repos
/enterprise-architecture-review --domain ansible-collections
/enterprise-architecture-review --domain infra-repos
/enterprise-architecture-review --domain app-fleet

# Single project / repo review
/enterprise-architecture-review docker_servers
/enterprise-architecture-review /Users/wernervandermerwe/Documents/git_repos/IOTel/netsapiens-ansible

# Depth selectors (default: --deep)
/enterprise-architecture-review --workspace --standard   # 4 consolidated boards, faster
/enterprise-architecture-review --workspace --fast       # single super-reviewer, smoke check only

# Skip embedded accuracy gate (rarely advised)
/enterprise-architecture-review --workspace --no-accuracy-gate

# Weekly status refresh mode (graph only)
/enterprise-architecture-review --workspace --roadmap-graph-only

# Explicit full review mode (recommended for monthly/quarterly runs)
/enterprise-architecture-review --workspace --full-review
```

`--roadmap-graph-only` is intended for weekly project status refreshes. It ingests Jira board/backlog and current roadmap files, then updates `ROADMAP_GRAPH.md` only (including Jira flow, cross-epic dependencies, and epic graphs in priority order). It does not regenerate the full review artifacts.

`--full-review` forces the complete pipeline and skips mode-selection prompting.

Jira data source policy:

- For Jira reads/writes, use Atlassian MCP Jira tools only.
- Preferred issue retrieval tool: `mcp_atlassian_atl_searchJiraIssuesUsingJql`.
- Use `mcp_atlassian_atl_getJiraIssue` for ticket detail expansion when needed.
- Do not use direct shell `curl` Jira API calls when MCP Jira tools are available.
- Execute Jira MCP reads/writes sequentially, one request at a time. Do not parallelize Jira MCP searches or mutations in this skill.

## Core Operating Principles

1. **No sacred cows.** No technology, framework, abstraction, or historical decision is exempt.
2. **System-level thinking.** Repos are reviewed in the context of the platform, not in isolation.
3. **Adversarial by design.** Each board actively tries to invalidate the platform's assumptions.
4. **Evidence over opinion.** Every finding references specific files, paths, symbols, or runtime behavior.
5. **2am test.** A tired, unfamiliar engineer must be able to debug and recover the system at 2am. Failures here are CRITICAL.
6. **Operational simplicity > theoretical correctness.** Fewer moving parts win.
7. **Rebuild determinism is non-negotiable.** Quarterly rebuild must produce the same platform.
8. **Standards must be enforceable.** A standard that linting cannot validate is aspirational, not real.
9. **Findings carry actions.** A finding without a remediation roadmap is incomplete.
10. **Documentation must match reality.** Drift between docs and code is itself a finding.

The full principle set lives in `references/review_principles.md`.

## Pipeline

```
Stage 0   Accuracy Gate         (doc-vs-code drift; mandatory unless --no-accuracy-gate)
Stage 1   Design Viability      (kill switch — STOP / PROCEED / PROCEED-WITH-CONCERNS)
Stage 2   Specialist Boards     (5-9 parallel subagents, isolated, controlled disagreement)
            Batch 1  Security & Trust
            Batch 2  Infrastructure & Rebuildability
            Batch 3  Operability & Reliability             [HARD GATE]
            Batch 4  Engineering & Quality
            Batch 5  Performance & Scalability
            Batch 6  Developer Experience & Cognitive Load
            Batch 7  Platform Strategy & Technology Evolution
            Batch 8  Cost, Efficiency & Sustainability     [DECISION GATE]
            Batch 9  Documentation Quality                 [emits ready-to-apply edits]
Stage 3   Referee Consolidation (de-dupe, systemic patterns, top-N roadmap, scorecard)
Stage 3b  Roadmap Reality Reconciliation (re-check NEXT_TICKETS/NEXT_EPICS against code reality)
Stage 3c  Jira IO Alignment      (read IO board/backlog, align priorities and in-flight work)
Stage 4   Artifact Generation   (quality-review, executive briefing, risk register, roadmap files, roadmap graphs)
Stage 4b  Doc-Fix Application   (prompted — apply Batch 9 edits to source documents)
```

Depth selector affects Stage 2 only:

- `--deep` (default): all 9 boards run in parallel. Required for quarterly/renewal cycles.
- `--standard`: 4 consolidated boards — Security+Trust, Infra+Reliability, Engineering+Performance+DevEx+Docs, Strategy+Cost+Sustainability.
- `--fast`: a single super-reviewer with all lenses internalized, no parallelism. Smoke check only — never use for renewal artifacts.

## Output Contract

All output is written to files. Nothing strategic is left in chat. The chat response at the end is a short pointer to the artifacts.

### Workspace scope (`--workspace`)

Written under `Architecture_Knowledge_Base/`:

| Artifact | Path |
|---|---|
| Quality review (strategic narrative) | `quality-reviews/workspace_architecture_review-YYYY-MM-DD.md` |
| Executive briefing (1-2 page, stakeholder-friendly) | `quality-reviews/EXECUTIVE_BRIEFING-YYYY-MM-DD.md` |
| Risk register (markdown table) | `quality-reviews/RISK_REGISTER-YYYY-MM-DD.md` |
| Risk register (CSV — trendable) | `quality-reviews/risk_register-YYYY-MM-DD.csv` |
| Roadmap — epics | `roadmap/NEXT_EPICS.md` *(updated in place; new epics merged in priority order)* |
| Roadmap — tickets | `roadmap/NEXT_TICKETS.md` *(updated in place)* |
| Roadmap — graphs | `roadmap/ROADMAP_GRAPH.md` *(updated in place; multi-graph Mermaid regenerated)* |

### Domain scope (`--domain <name>`)

Same artifacts, written under `Architecture_Knowledge_Base/quality-reviews/<domain>/` and merged into the AKB roadmap files (since a domain crosses repos and has no single owning repo).

### Project / repo scope (`<project-path>`)

| Artifact | Path |
|---|---|
| Quality review | `<project>/architecture/quality-reviews/ARCHITECTURE_REVIEW-YYYY-MM-DD.md` |
| Executive briefing | `<project>/architecture/quality-reviews/EXECUTIVE_BRIEFING-YYYY-MM-DD.md` |
| Risk register (md + csv) | `<project>/architecture/quality-reviews/RISK_REGISTER-YYYY-MM-DD.{md,csv}` |
| Roadmap tickets | `<project>/roadmap/NEXT_TICKETS.md` *(created if absent)* |
| Roadmap graphs | `<project>/roadmap/ROADMAP_GRAPH.md` *(created if absent)* |

If the project lacks an `architecture/quality-reviews/` directory, create it.

## Roadmap Update Rules (NEXT_TICKETS.md / NEXT_EPICS.md)

The roadmap files are mission-critical. These rules are strict and enforced by Stage 4:

1. **Completed tickets are preserved.** Never delete a row that has state `✅ Done`. Existing checkbox-style `[x] Done` entries are normalized to the visual labels but kept.
2. **In-flight tickets may be updated in place.** Replace, split, merge, or rewrite only when superseded; record the reason in the row or an adjacent note.
3. **State labels (Confluence-friendly visual labels — no checkboxes):**
   - `✅ Done`
   - `🟡 In Progress`
   - `⬜ Planned`
   - `⛔ Blocked`
   - `⏸ Deferred`
4. **Sprint sizing (hours):** every ticket must be recorded in hours (`h`) and include mandatory admin overhead (`+1h` for half-day work, `+2h` for full-day work). Any ticket above `20h` must be split before it can be added.
5. **Priority bands:** P0 (must-do this cycle), P1, P2, P3, P4. New findings land in the band the referee assigns.
6. **Ticket cap:** at most 50 tickets in `NEXT_TICKETS.md`, sorted by priority. Lower-priority items get demoted, not deleted, when the cap is exceeded — the demotion target is `NEXT_EPICS.md` if they remain strategic, otherwise the run notes.
7. **NEXT_EPICS.md** holds candidate epics — items larger than `20h` that still need refinement. The skill appends new epics in priority order; existing epics are updated in place.
8. **Roadmap graph page:** `ROADMAP_GRAPH.md` is regenerated each run and is the primary talking-point artefact for roadmap review meetings.
9. **Graph structure rule:** `ROADMAP_GRAPH.md` contains multiple Mermaid graphs (one per epic) plus a first cross-epic dependency graph that includes epic-to-epic and epic-to-ticket dependencies.
9a. **Jira graph rule:** `ROADMAP_GRAPH.md` must include a Jira flow graph above cross-epic dependencies to show Jira board/backlog status flow, derived priority ordering, and weekly execution path.
10. **Last-updated date** at the top of each file is set to today.
11. **Implicit epic intake (default ON):** epics in `NEXT_EPICS.md` must be re-scored each run. High-priority epics are automatically decomposed into ticket-sized units and pulled into `NEXT_TICKETS.md` unless the user explicitly requests manual-only epic inclusion.
12. **Ticket reality reconciliation (mandatory):** before finalizing `NEXT_TICKETS.md`, compare each active ticket with current code/docs/runtime evidence. If definition-of-done is already met, mark as `✅ Done` even when the previous state was stale.
13. **Jira IO board/backlog input (mandatory):** read current Jira IO board and backlog as part of prioritization. Use Jira issue keys (`IO-###`) where available in roadmap rows.
14. **Jira status semantics:** if a Jira issue is in Backlog or To Do, include it in prioritization; treat To Do as currently in-flight human/MCP work and avoid conflicting duplicate ticket creation.
15. **Jira Epic hygiene (default ON):** every active Jira ticket in review scope must have an Epic parent/link. If missing, auto-assign to an open fitting Epic; if no open Epic fits, create a new Epic and assign the ticket.

Roadmap decision flow reference:

- `references/roadmap_decision_flow.md` (Mermaid graph + ordered checklist of inputs and routing)

## Modular Lens-Agent Pattern

To make tuning easier over time, this skill supports a modular lens-agent pattern:

- Keep this skill as the orchestrator and contract owner.
- Tune individual lens behavior in dedicated lens-agent spec files.
- Keep consolidation and roadmap routing centralized in the referee stage.

Starter lens-agent specs:

- `references/lens-agents/OPERABILITY_RELIABILITY.md`
- `references/lens-agents/COST_SUSTAINABILITY.md`
- `references/lens-agents/DOCUMENTATION_QUALITY.md`

Supporting docs:

- `references/lens_agents_orchestration.md` (parallel invocation model + guardrails)
- `templates/lens-agents/OUTPUT_SCHEMA.md` (required shared output contract)
- `templates/lens-agents/LENS_AGENT_TEMPLATE.md` (new lens bootstrap template)

## Execution Steps

The execution model below is what this skill performs when invoked. The full prompts for each stage are in `prompts/`.

### Step 1 — Resolve scope

Parse `$ARGUMENTS`. Resolve in this order:

0. Resolve execution mode first.
1. If `--roadmap-graph-only` is present: run weekly graph-only mode.
2. Else if `--full-review` is present: run full review mode.
3. Else prompt user first (mandatory): `Choose review mode: Full review or Jira reflection update only`.
  - This must be the first interactive question in the run.
  - Do not ask scope/output-path prompts before this question.
  - Do not infer mode from previous runs.
4. Map prompt result:
  - `Full review` -> full pipeline
  - `Jira reflection update only` -> `--roadmap-graph-only` behavior

Then resolve scope in this order:

1. Empty or `--workspace` → workspace scope.
2. `--domain <name>` → load the domain definition from `references/domains.md`. If the domain is unknown, ask the user to confirm member repos.
3. Existing directory path → project scope.
4. Bare token → match against project names in the parent workspace. One match wins; multiple matches require disambiguation.

Empty resolved file set → stop with error.

### Step 2 — Load skill assets

Read everything under `references/` and `prompts/` once. These are passed to every subagent so all boards share the same definitions of severity, finding schema, scoring, and technology context.

### Step 3 — Stage 0: Accuracy Gate (unless `--no-accuracy-gate`)

Launch one subagent with `prompts/stage-0-accuracy.md`.

Input:
- scope and file set
- the relevant AKB documents (`design/`, `standards/`, `runbooks/`, `decisions/`, `architecture/` if any)

Output:
- per-document accuracy verdict (PASS / PASS-WITH-NOTES / FAIL)
- list of `outdated`, `unproven`, `ambiguous` claims with evidence and corrections
- a documentation-drift signal that the referee treats as a first-class finding category

If accuracy gate verdict is FAIL for a critical document, the referee will flag the platform as **doc-drift critical** even if the code is sound.

### Step 4 — Stage 1: Design Viability Gate

Launch one subagent with `prompts/stage-1-viability.md`.

Possible verdicts:
- `STOP` → halt. Write a brief halt report explaining why detailed review is wasteful. Do not launch boards.
- `PROCEED` → continue.
- `PROCEED-WITH-CONCERNS` → continue, but forward the listed concerns to every board as additional context.

### Step 5 — Stage 2: Specialist Boards (parallel)

Depth `--deep` (default) launches 9 isolated subagents in parallel. Each receives:

- scope and file set
- review mode (`code-only` vs `code-plus-runtime` — ask if runtime artifacts like OpenVAS / OpenSCAP / SIEM exports are available)
- `references/technology_context.md`
- `references/severity_rubric.md`
- `references/finding_schema.md`
- `references/scoring_model.md` (each board scores its own categories)
- Stage 0 accuracy gate signals
- Stage 1 viability concerns

Board prompts live in `prompts/batch-N-*.md`. Each board has **explicit non-ownership** to prevent finding duplication. The full ownership matrix is in `references/board_ownership.md`.

Hard gates and special outputs:
- **Batch 3** must produce an explicit *operability verdict*. A failed operability gate is a CRITICAL platform-level finding.
- **Batch 8** must produce a *keep / simplify / remove / defer* decision per major subsystem. No general findings.
- **Batch 9** must produce *ready-to-apply edits* for straightforward textual fixes plus *manual rewrite* entries for findings requiring author judgment. These edits drive Step 7b.

No board sees another board's output.

### Step 6 — Stage 3: Referee Consolidation

Launch one subagent with `prompts/referee.md`. Input is:

- Stage 0 output
- Stage 1 output
- all board outputs
- `templates/quality_review.md`
- `templates/executive_briefing.md`
- `templates/risk_register.md`
- `templates/next_tickets.md`
- `templates/next_epics.md`
- `templates/mermaid_roadmap.md`
- `references/scoring_model.md`

Referee responsibilities:

1. **De-duplicate** findings across boards. The same issue surfacing in multiple boards is upgraded in severity, not listed multiple times.
2. **Identify systemic patterns** — recurring themes across boards (e.g. "duplication-as-truth", "implicit ordering", "the vault_kv-shaped hole"). These are first-class output sections.
3. **Score** the platform across the weighted categories (Security 20%, Reliability 20%, Maintainability 15%, OpEx 15%, Architecture 15%, DevEx 10%, Cost 5%). Each score includes a narrative explanation.
4. **Produce top-N priority roadmap.** Default top-20 for workspace, top-10 for project, top-10 for domain.
5. **Resolve disagreement.** When boards disagree, the referee explicitly records the disagreement and the chosen path with rationale.
6. **Final verdict block.** Six yes/no questions, each with one-sentence justification (understandable? ownership clear? operationally sustainable? over-engineered? rebuildable? onboardable?).

### Step 6b — Stage 3b: Roadmap Reality Reconciliation

Run one reconciliation pass before writing roadmap files.

Inputs:

- current `NEXT_TICKETS.md`
- current `NEXT_EPICS.md`
- roadmap planning documents under `roadmap/` (for example `FIREWALL_POLICY.md`, `MONITORING_STACK.md`, `SECURITY_SCANNING_MODERNIZATION.md`, `VAULT_EXECUTION_CONTEXT.md`), excluding generated views like `ROADMAP_GRAPH.md`
- referee-ranked actions from Stage 3
- current code/docs evidence from scope

Required actions:

1. **Ticket validity check:** verify each active ticket still maps to unresolved work.
2. **Stale closure correction:** if a ticket appears completed in code/docs, mark it `✅ Done` with evidence note.
3. **Obsolete work handling:** if work is no longer applicable, keep historical row but mark as deferred/superseded with reason.
4. **Epic intake:** re-score all open epics by importance and urgency.
5. **Automatic promotion:** for high-priority epics, generate ticket-sized breakdown items (<=20h, admin overhead included) and insert them into priority bands.
6. **Manual override support:** if user requests explicit epic inclusion only, skip automatic promotion and leave epic as candidate.
7. **Roadmap intent ingestion:** incorporate unresolved or upcoming work described in `roadmap/*.md` planning documents into prioritization, then map to `NEXT_TICKETS.md` or `NEXT_EPICS.md` using effort and policy rules.

### Step 6c — Stage 3c: Jira IO Alignment

Read and align against Jira before writing roadmap files.

Inputs:

- Jira IO board issues
- Jira IO backlog issues
- Jira issue status and key metadata
- reconciled roadmap outputs from Stage 3b
- roadmap planning documents under `roadmap/` so Jira alignment does not conflict with documented roadmap intent

Jira retrieval requirement:

- Fetch Jira input via Atlassian MCP Jira tools (`mcp_atlassian_atl_searchJiraIssuesUsingJql`, `mcp_atlassian_atl_getJiraIssue`) and treat this as the source of truth for status.
- Use Atlassian MCP Jira mutation tools for Epic hygiene (`mcp_atlassian_atl_getJiraIssueTypeMetaWithFields`, `mcp_atlassian_atl_createJiraIssue`, `mcp_atlassian_atl_editJiraIssue`).
- Read Jira MCP data sequentially: board snapshot, backlog snapshot, closed-last-2-weeks snapshot, then detail expansion only where needed.
- Apply Jira mutations sequentially and re-check state after each batch rather than issuing parallel updates.

Required actions:

1. **Issue-key mapping:** map `NEXT_TICKETS.md` rows to Jira keys where possible; preserve existing keys.
2. **Backlog consideration:** include Jira Backlog items in priority evaluation for new/updated roadmap entries.
3. **To Do treatment:** treat Jira To Do items as in-flight work; keep them visible and prioritized without creating duplicate competing tickets.
4. **Duplicate suppression:** if roadmap row and Jira issue represent same work, merge references instead of adding a new duplicate row.
5. **Status reconciliation:** if Jira indicates clear completion but roadmap is stale, mark roadmap row `✅ Done` with evidence note.
6. **Epic assignment enforcement (automatic):** for active Jira tickets missing an Epic parent/link, assign an open fitting Epic based on title/label/roadmap context.
7. **Epic creation fallback (automatic):** if no open Epic reasonably fits, create a new Epic in project IO and assign the ticket to it.
8. **Field resolution safety:** resolve Jira Epic-link field dynamically from project/issue-type metadata before updates (do not hardcode field IDs).

### Step 7 — Stage 4: Artifact Generation

The main thread (this skill, not a subagent) writes all artifacts. Subagents propose content; the main thread writes files.

Generation order:

1. **Quality review** (full strategic narrative). Path per scope rules above.
2. **Executive briefing** (1-2 page, plain language, stakeholder-suitable).
3. **Risk register** (markdown table + CSV). CSV header matches `references/finding_schema.md` for easy trending across runs.
4. **NEXT_EPICS.md** update. New epics merged; existing epics updated in place; completed items preserved.
5. **NEXT_TICKETS.md** reconciliation update. Existing ticket rows validated against current code/docs evidence and stale state corrected where required.
6. **NEXT_TICKETS.md** roadmap update. New tickets in priority bands (including auto-promoted epic breakdown items); ticket cap enforced.
7. **Jira reference update.** Ensure roadmap rows include Jira IO keys where known and remove duplicate overlap with Jira To Do/Backlog issues.
8. **ROADMAP_GRAPH.md** update. Regenerate multi-graph Mermaid content in this order: Jira flow-and-priority graph, cross-epic dependency graph, then one graph per epic grouped under `Epic Graphs` and ordered by epic priority.

### Step 7c — Weekly Graph-Only Mode (`--roadmap-graph-only`)

When `--roadmap-graph-only` is present, run a reduced path focused on status-meeting visuals:

0. Do not ask for report output location in this mode.

1. Read current Jira IO board and backlog via Atlassian MCP Jira tools, sequentially one query at a time.
2. Read `NEXT_TICKETS.md`, `NEXT_EPICS.md`, and roadmap planning docs under `roadmap/`.
3. Reconcile in-flight/done status for graph display only.
4. Validate active Jira tickets for metadata hygiene: each active ticket must have an Epic parent and/or at least one existing label; report compliance and remediation counts as text bullets above the Jira graph.
5. Extract and render cross-person dependencies explicitly in the active flow (for example phase-ticket chain dependencies on another engineer's blocking ticket such as `IO-199`).
6. Auto-remediate Epic hygiene during weekly run: assign missing Epic links and create new open Epic entries when no fit exists.
7. Regenerate `ROADMAP_GRAPH.md` only, in this order:
  - Jira weekly catchup flow graph with:
    - first block: all tickets closed in the last 2 weeks (ID + title)
    - active flow: current In Progress/To Do tickets grouped by assignee with recommended execution order
    - cross-person dependency edges between assignee lanes
    - ticket-only nodes (do not include non-ticket governance nodes in Mermaid)
    - final block: one single backlog box
    - plus text bullets above the graph for Epic hygiene coverage and auto-remediation summary
  - Cross-epic and ticket dependency graph
  - `## Epic Graphs` section with one epic execution-sequence graph ordered by current epic priority
8. Do not regenerate quality review, executive briefing, risk register, `NEXT_TICKETS.md`, or `NEXT_EPICS.md` in this mode.
9. After graph regeneration, prompt publish scope:
  - `Publish Roadmap page only` (default) -> run `./publish_to_confluence.sh --roadmap-only`
  - `Publish all managed pages` -> run `./publish_to_confluence.sh`
  - `Do not publish now` -> skip publishing

### Step 7b — Stage 4b: Documentation Fix Application (prompted)

This step is the **only** place where the skill modifies source documents. It is prompted, not autonomous. Modifying source documents is fundamentally different from writing review artifacts: the user must opt in.

If Batch 9 produced ready-to-apply edits:

1. Summarize the edits in chat:
   - count by severity
   - count by document
   - count of `operation: manual` items (cannot be auto-applied)
2. Ask the user:

   > "Apply documentation fixes directly? Respond `yes` (CRITICAL/HIGH only, default), `all` (also MEDIUM/LOW), or `no` (keep findings in roadmap and risk register only)."

3. On `yes`: apply edits with severity ∈ `{CRITICAL, HIGH}` whose `operation` is not `manual`. On `all`: include `MEDIUM`/`LOW`. On `no`: skip and keep all findings as roadmap/register entries.
4. Apply edits via `Edit` tool calls against each source file. Edits must use the exact `anchor` text as the search string.
5. After application, write a summary file at:
   - `<scope-root>/quality-reviews/DOC_FIXES_APPLIED-YYYY-MM-DD.md`
   - listing every file edited, the edits applied, the edits skipped, and the `operation: manual` items left for the author
6. CRITICAL/HIGH Batch 9 findings whose `operation` is `manual` (require structural rewrite) still flow into `NEXT_TICKETS.md` or `NEXT_EPICS.md` per the roadmap rules. Doc fixes that *were applied* are removed from the roadmap (they no longer exist as open work).

If Batch 9 produced **no** edits, Step 7b is skipped silently.

### Step 8 — Chat response

The final chat response is short and must include:

- the absolute paths of every artifact written
- the Stage 1 viability verdict
- the Batch 3 operability gate result
- the Batch 8 sustainability decisions count
- the Batch 9 documentation verdict and doc-fix outcome (applied / declined / none)
- counts by severity (CRITICAL / HIGH / MEDIUM / LOW)
- the top 3 outage risks
- the top 3 simplification wins
- the top 3 documents that fail their reader
- the 30-day priority sequence

Nothing else. The artifacts carry the detail.

## Prioritization Weighting (for the referee's roadmap ordering)

For non-internet-facing internal platforms with a small SRE team:

1. Operability and debuggability under pressure
2. Reliability, rebuild safety, recovery determinism
3. Simplicity and cognitive load reduction
4. Pattern consistency under engineer churn
5. Security posture and access controls

Public-facing systems flip 1↔5. Confirm review mode in Stage 1 before applying weighting.

## Anti-Goals

This skill **must not** become any of the following. If you find yourself drifting into one, stop and reconsider.

- a linting exercise
- a style-enforcement mechanism
- a compliance checkbox process
- a bureaucracy generator
- an approval rubber stamp
- a vanity-metrics scorecard
- a blocker to engineering progress

A finding that says "this could be slightly nicer" is noise. A finding that says "this will fail at 2am during a partial Vault outage and here is the file:line that breaks" is signal.

Prefer signal.

## Success Criteria

A successful run produces:

1. One dated quality-review document with evidence-backed findings.
2. One executive briefing suitable for a non-technical reader.
3. One risk register (md + csv) capable of trending across runs.
4. An updated NEXT_TICKETS.md.
5. An updated ROADMAP_GRAPH.md with regenerated Mermaid graphs.
5. An updated NEXT_EPICS.md with new strategic epics.
6. Stage 0 accuracy outcome recorded.
7. Stage 1 viability outcome recorded.
8. Stage 2 hard gate (operability) and decision gate (sustainability) outcomes recorded.
9. Top-N priority roadmap that is strictly ordered and shippable.
10. A short, actionable chat summary that links to every artifact.

## Final Chat Response Format

```
Review complete.

Scope: <workspace | domain:<name> | project:<name>>
Depth: <deep | standard | fast>
Date:  YYYY-MM-DD

Stage 1 viability:        <PROCEED | PROCEED-WITH-CONCERNS | STOP>
Stage 2 operability gate: <PASS | FAIL>
Stage 2 sustainability:   <N keep / N simplify / N remove / N defer>
Stage 2 documentation:    <N docs reviewed / N serves / N partial / N fails>
                          doc-fix: <N applied | declined | not-prompted>

Findings by severity:
  CRITICAL: <n>   HIGH: <n>   MEDIUM: <n>   LOW: <n>

Top 3 outage risks:
  1. <title>
  2. <title>
  3. <title>

Top 3 simplification wins:
  1. <title>
  2. <title>
  3. <title>

Top 3 reader failures (documents):
  1. <document path>
  2. <document path>
  3. <document path>

30-day priority sequence:
  1. <ticket id> — <title>
  2. <ticket id> — <title>
  ...

Artifacts:
  - <abs path to quality review>
  - <abs path to executive briefing>
  - <abs path to risk register md>
  - <abs path to risk register csv>
  - <abs path to NEXT_TICKETS.md>
  - <abs path to ROADMAP_GRAPH.md>
  - <abs path to NEXT_EPICS.md>
  - <abs path to DOC_FIXES_APPLIED-YYYY-MM-DD.md>   (only if Step 7b ran)
```
