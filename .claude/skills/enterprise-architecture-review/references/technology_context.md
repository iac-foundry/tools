# Technology Context — IOTel Platform

This file is loaded by every board and the referee. It is the shared baseline so no board has to re-discover the platform from scratch.

Keep this file current when material technology choices change. Significant changes warrant a LADR in `decisions/`.

## Platform Shape

The IOTel workspace is a single operational platform composed of multiple repositories. It is **not** reviewed repo-by-repo; it is reviewed as one system.

Roles within the platform:

- **Architecture knowledge base** — `Architecture_Knowledge_Base/` (this repo). Single source of truth for design, standards, runbooks, decisions, roadmap.
- **Ansible collections** — `iotel-ansible-common`, `iotel-ansible-monitoring`, `iotel-ansible-security`. Shared role library; pinned via `requirements.yml` in every consumer.
- **Infrastructure repos** — `docker_servers`, `infra-sec-01`, `infra-apt-01`, `tfc_configuration`, `ubuntu_vm-vsphere`, `ubuntu-vm-templates`. Terraform-Cloud-backed; Jenkins-driven.
- **Application repos** — `netsapiens-ansible`, `simblecrm`, `iotel-identity`, `whatsapp_ai_bot-ansible`, `agentx-ansible`. Consumers of the collections.
- **One-offs** — `iotel_terraform_pet` (pet VMs), `tools` (project scaffolding, shared scripts).

## Core Topology

Three orchestration hosts form the operational core: `infra_orch_01`, `infra_orch_02`, `infra_orch_03`. Each runs **on the host OS**:

- Docker
- Vault Agent on `127.0.0.1:8100`
- DNSMasq

Inside Docker, each ORCH host runs a **Jenkins agent container**. The agent container reaches Vault through the host's Vault Agent (host-network or container-to-host bridge). The agent container does **not** have its own Vault connection.

`infra_orch_01` hosts the Jenkins controller and is a **bootstrap-critical special case**. To avoid circular rebuild dependency, ORCH-01 provisioning lives in a dedicated TFC workspace.

## Approved Secret Paths

Only these two paths are sanctioned. Anything else is a finding.

```
Path 1 (Ansible consumers):
  Host Vault Agent → Jenkins agent container → lookup('iotel.common.vault_kv', ...) in playbook

Path 2 (non-Ansible pipeline stages):
  Host Vault Agent → Jenkins agent container → vaultEnv {} shared-lib step → TF / Packer / shell
```

See `standards/VAULT_CONSUMPTION.md` and `design/VAULT_OPERATING_MODEL.md`.

## Tooling Stack (current)

| Layer | Tool | Notes |
|---|---|---|
| IaC | Terraform via Terraform Cloud | LADR-001 |
| Secrets | HashiCorp Vault + Vault Agent | LADR-002 |
| SCM | Bitbucket | LADR-003 |
| CI/CD | Jenkins | LADR-004 |
| Identity | Authentik | LADR-005 |
| Config Mgmt | Ansible (collections) | |
| Hypervisor | vSphere | |
| Image build | Packer | |
| Monitoring | Telegraf → Graylog → Graphite | in rollout |
| Compliance scan | OpenSCAP | epic, planned |
| Vuln scan | OpenVAS / GVM | epic, planned |
| Logging | Graylog | |
| OS | Ubuntu 24.04 LTS | template `linux-ubuntu-24.04-lts-HEAD` |

## Trust Model — three zones

The platform applies a three-zone trust model. Specific names live in design docs; the key property is:

- crossing zones requires explicit, audited authorization
- cross-zone secret reads route through Vault — never direct
- pipelines may operate across zones only via the documented agent labels

## Known Architectural Tensions (carry forward as context — confirm vs current code)

These are systemic patterns surfaced in the 2026-05-08 workspace review. Every new review must check whether each one has been retired, persists, or has worsened.

1. **The `vault_kv`-shaped hole** — the lookup plugin referenced in standards may or may not exist in `iotel-ansible-common/plugins/lookup/`. Until it exists and is consumed, every host-side Vault violation traces back to its absence.
2. **State ownership confusion at the TF↔Ansible boundary** — cloud-init `write_files`, inventory generation, and Packer image identity all leak.
3. **Duplication-as-truth** — host IPs, `validate.sh`, `ansible.cfg`, `twingate_connector`, Vault address aliases, TFC workspace names.
4. **Implicit ordering** — roles depend on prior plays; `meta/main.yml` does not declare these dependencies.
5. **App repos at very different distance-to-standard** — `agentx-ansible` closest; `simblecrm` furthest.
6. **Scaffolding propagates yesterday's drift** — `tools/new_project` ships small amounts of debt.

Verify these against the current code each run. Confirm-or-retire is itself a finding.

## Useful Conventions

- File naming follows `TECHNOLOGY_CONTEXT` (see `standards/DOCUMENTATION.md`).
- Decisions follow `LADR-NNN-decision-title.md`.
- Quality reviews are dated: `workspace_architecture_review-YYYY-MM-DD.md`.
- All ticket rows in `NEXT_TICKETS.md` are ≤ 20h (including admin overhead).
- Confluence labels: `akb-managed` on every Confluence-published page; Confluence is generated from the repo, never edited directly.

## When to Re-Generate This Context

- After a LADR is accepted or superseded
- After a major tooling addition or replacement
- After a topology change (e.g. orchestration host count)
- When a board reports that the context contradicted observed code
