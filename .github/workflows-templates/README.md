# Blueprints CI/CD workflow templates

Reusable GitHub Actions templates for each extracted `ansible-collection-<name>` repo. They are kept
here (not under `.github/workflows/`) so they are **templates**, not active workflows on the
incubation monorepo.

When a collection is extracted to its own repo:

1. Copy `ci.yml` and `release.yml` into that repo's `.github/workflows/`.
2. Vendor `collections/ci/no_hidden_deps_guard.sh` into the repo as `ci/no_hidden_deps_guard.sh`.

## Gates (both workflows)

1. **no-hidden-dependencies guard** — enforces
   [BLUEPRINTS_DESIGN_PRINCIPLES.md](../../docs/design/BLUEPRINTS_DESIGN_PRINCIPLES.md) §1/§2.
2. **ansible-lint** — production profile.
3. **molecule** — converge + idempotence per role (`ci.yml`).

`release.yml` additionally builds the collection and publishes it via the configured channel
(GitHub release artifact and/or Galaxy/Automation Hub) per
[BLUEPRINTS_GALAXY_PUBLISHING.md](../../docs/standards/BLUEPRINTS_GALAXY_PUBLISHING.md).
