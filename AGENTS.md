# AGENTS.md

Guidance for AI agents and automated contributors working in this repository.

## Project overview

This repository is a lean development container template for GitHub Codespaces
and local VS Code Dev Containers. It defines a reproducible Ubuntu 26.04
environment with Python 3, Node.js LTS, GitHub CLI, Claude Code CLI, and
OpenCode TUI. The repository contains no application source code — all
meaningful files are configuration.

## Repository layout

```
.devcontainer/
  Dockerfile          Base image pin + apt package installs (python3, venv, pip, sudo)
  devcontainer.json   Feature declarations, VS Code extensions, volume mounts, lifecycle commands
.github/
  dependabot.yml      Daily Dependabot updates for GitHub Actions, Docker images, devcontainer features
  workflows/
    ci.yml            Single CI pipeline: build → test → scan (see CI section below)
.gitignore            Excludes OS files, editor dirs, Claude session state (.claude/, CLAUDE.md)
AGENTS.md             This file
LICENSE               MIT
README.md             Human-facing documentation
```

## Conventions

### Commit messages
Follow Conventional Commits as used throughout the repo history:
- `feat(devcontainer):` — devcontainer features, base image, extensions
- `ci:` / `ci(deps):` — workflow changes, dependency bumps
- `docker:` — Dockerfile or base image changes
- `docs:` — README, AGENTS.md, comments

### JSON
`devcontainer.json` is **strict JSON** (no comments, no trailing commas).
Validate with `python3 -m json.tool .devcontainer/devcontainer.json` before
committing.

### GitHub Actions
All `uses:` entries **must** be pinned to a full 40-character commit SHA with a
`# vX.Y.Z` or `# vX` trailing comment, e.g.:
```yaml
uses: actions/checkout@9c091bb21b7c1c1d1991bb908d89e4e9dddfe3e0 # v7.0.0
```
Never use a floating tag (`@main`, `@v4`, `@latest`). Dependabot keeps SHAs
current via daily PRs.

### Dockerfile
- One `FROM` line; base image tag must be a versioned MCR tag so Dependabot
  can track it (e.g. `ubuntu26.04`, `ubuntu-24.04`; note MCR dropped the
  hyphen for 26.04+).
- `hadolint` runs in CI; respect its rules or add a targeted
  `# hadolint ignore=DLXXXX` with a justification comment.
- Keep the package list minimal — features (not the Dockerfile) install
  language runtimes and CLI tools.

## CI pipeline (`ci.yml`)

Three sequential jobs share a single Docker build via artifact:

| Job | `needs` | What it does |
|-----|---------|-------------|
| `build` | — | hadolint → gitleaks (SARIF) → `docker build` → `docker save` → upload artifact |
| `test` | `build` | Full devcontainer build via `devcontainers/ci`; smoke-tests all required binaries |
| `scan` | `build` | `docker load` → Trivy image scan (gates on CRITICAL) → SBOM → Trivy fs scan (gates on CRITICAL) |

Triggers: `push`/`pull_request` on `main`, daily `schedule` (02:00 UTC),
`workflow_dispatch`.

**SARIF uploads** are skipped for fork PRs (read-only token) via:
```yaml
if: >
  always() &&
  (github.event_name != 'pull_request' ||
   github.event.pull_request.head.repo.full_name == github.repository)
```

## Local validation commands

Run these before committing, in order:

```bash
# 1. JSON validity
python3 -m json.tool .devcontainer/devcontainer.json > /dev/null

# 2. Dockerfile lint
docker run --rm -i hadolint/hadolint < .devcontainer/Dockerfile

# 3. Base image build (mirrors ci.yml build job)
docker build --pull -t dev-template:local .devcontainer/

# 4. Full devcontainer build + smoke test (mirrors ci.yml test job)
#    Requires network egress to ghcr.io and registry.npmjs.org
npx -y @devcontainers/cli up --workspace-folder . --remove-existing-container
npx @devcontainers/cli exec --workspace-folder . -- bash -c '
  set -eu
  for cmd in python3 node npm gh opencode curl jq claude; do
    command -v "$cmd" || { echo "MISSING: $cmd"; exit 1; }
  done
  claude --version
  echo "All checks passed"
'
```

## Guardrails

- **Keep the CI smoke test in sync.** If you add a new tool via a feature or
  `updateContentCommand`, add it to the `for cmd in ...` loop in `ci.yml`
  (`test` job, `runCmd` block).
- **No secrets in tracked files.** `.claude/` and `CLAUDE.md` are gitignored;
  do not commit authentication tokens, API keys, or credential files.
- **Do not remove SHA pins.** Floating action refs will fail code review.
- **Do not push directly to `main`.** All changes go through a PR from a
  feature branch.
- **Volume mount target.** The Claude Code volume is mounted at
  `/home/vscode/.claude` (matching `remoteUser: vscode`). If the remote user
  ever changes, the mount target must be updated to match.
