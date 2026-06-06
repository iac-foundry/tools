# iac-foundry/tools

Shared CI tooling for the `blueprints.*` composable platform collections.

## Contents

### `ci/no_hidden_deps_guard.sh`

Enforces [Design Principles §1 and §2](https://github.com/iac-foundry/docs/blob/main/design/BLUEPRINTS_DESIGN_PRINCIPLES.md):
- No secret retrieval inside component role tasks (`vault kv`, `community.hashi_vault`)
- No cross-collection `include_role` inside component collections

Every `blueprints.*` component collection vendors this script at `ci/no_hidden_deps_guard.sh`
and invokes it as the first step of every CI run.

### `.github/workflows-templates/`

Reusable GitHub Actions templates. Copy into a collection repo as `.github/workflows/`:

| Template | Purpose |
|---|---|
| `ci.yml` | PR gate: guard → ansible-lint → molecule |
| `release.yml` | Version tag: guard → lint → build → GitHub release + optional Galaxy publish |

The templates reference `bash ci/no_hidden_deps_guard.sh .` — the guard script must be
vendored in each collection repo for the workflow to function.

## Usage

When setting up a new `blueprints.*` collection repo:

```bash
# 1. Vendor the guard script
curl -sSL https://raw.githubusercontent.com/iac-foundry/tools/main/ci/no_hidden_deps_guard.sh \
  -o ci/no_hidden_deps_guard.sh && chmod +x ci/no_hidden_deps_guard.sh

# 2. Copy the CI workflow templates
mkdir -p .github/workflows
cp .github/workflows-templates/ci.yml      .github/workflows/ci.yml
cp .github/workflows-templates/release.yml .github/workflows/release.yml
```

Or simply copy both files from this repo when scaffolding a new collection.
