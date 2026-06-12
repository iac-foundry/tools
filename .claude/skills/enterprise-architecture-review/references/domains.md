# Domain Definitions

When `/enterprise-architecture-review --domain <name>` is invoked, the named domain expands to the listed repos and the skill scopes the review to that set.

Keep this file current when repos move between domains.

## ansible-collections

Shared role library — every consumer pins versions against these.

- `iotel-ansible-common`
- `iotel-ansible-monitoring`
- `iotel-ansible-security`

## infra-repos

Infrastructure provisioning and platform plumbing.

- `docker_servers`
- `infra-sec-01`
- `infra-apt-01`
- `tfc_configuration`
- `ubuntu_vm-vsphere`
- `ubuntu-vm-templates`

## app-fleet

Application repos that consume the collections.

- `netsapiens-ansible`
- `simblecrm`
- `iotel-identity`
- `whatsapp_ai_bot-ansible`
- `agentx-ansible`

## identity

Identity and access management surface.

- `iotel-ansible-security`
- `iotel-identity`
- relevant slices of `docker_servers` (Authentik deployment, Jenkins OIDC)

## monitoring

Telemetry, log, and metric flow.

- `iotel-ansible-monitoring`
- relevant slices of `docker_servers` (Graylog, Graphite)

## one-offs

Workspaces that don't fit a logical domain. Reviewed for either inclusion or retirement.

- `iotel_terraform_pet`
- `tools`

## Adding a Domain

To add a domain:

1. Append a new section here with the domain name and member repos.
2. Reference it in the skill invocation: `/enterprise-architecture-review --domain <name>`.
3. The skill resolves member paths relative to the IOTel workspace root (`/Users/wernervandermerwe/Documents/git_repos/IOTel/`).

## Domain-Review Output

Domain reviews write artifacts under `Architecture_Knowledge_Base/quality-reviews/<domain>/` (e.g. `quality-reviews/ansible-collections/ARCHITECTURE_REVIEW-2026-05-16.md`) and merge new tickets into the AKB workspace roadmap files, since a domain has no single owning repo.
