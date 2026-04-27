# Development Environment Repository

[![CI](https://github.com/idvoretskyi/dev/actions/workflows/ci.yml/badge.svg)](https://github.com/idvoretskyi/dev/actions/workflows/ci.yml)

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/idvoretskyi/dev)

This repository provides a lean development container template for GitHub
Codespaces and local VS Code Dev Containers.

## Features

- Ubuntu 24.04 base image
- Single-image devcontainer build based on `.devcontainer/Dockerfile`
- Core tooling for general development work:
  - Python 3 with `venv` and `pip`
  - Node.js LTS with `npm`
  - GitHub CLI
  - OpenCode TUI installed via `updateContentCommand` (cached by prebuilds)
  - Bash shell with common utilities
  - Build essentials (`gcc`, `make`, and related packages) via base image
- VS Code extensions:
  - Python and Pylance
  - GitHub Copilot and Copilot Chat
  - YAML

## Performance Optimizations

The devcontainer is tuned for fast Codespaces startup:

- Minimal feature set: only `common-utils`, `node`, and `github-cli` features
  are installed — Docker-in-Docker, sshd, and git features are omitted
- `common-utils` configured with zsh and Oh My Zsh disabled
- No full package upgrades during image build
- `curl`, `wget`, `jq`, and `git` sourced from the base image and
  `common-utils` feature — not re-installed in the Dockerfile
- OpenCode TUI installed in `updateContentCommand` instead of `postCreateCommand`,
  so it is cached during Codespaces prebuilds and not re-run on every start
- Small, targeted extension set

Estimated startup time: **45–75 seconds** without prebuilds,
**10–25 seconds** with prebuilds enabled (see below).

## Speeding Up Codespaces with Prebuilds

Codespaces prebuilds cache the fully built container image, installed features,
and `updateContentCommand` output on GitHub's infrastructure. This is the
single biggest lever for fast cold-starts.

### How to enable

1. Navigate to the repository on GitHub.
2. Go to **Settings → Codespaces**.
3. Click **Set up prebuild configuration**.
4. Select the `main` branch and choose one or more regions closest to your
   team.
5. Set the trigger to **"On every push"** (or "On configuration change" for
   less frequent rebuilds).
6. Save.

The first prebuild takes roughly 3–5 minutes. After that, every new Codespace
created from `main` will start in approximately **10–25 seconds**.

> **Tip**: Prebuilds are available on GitHub Team and Enterprise plans, and are
> free for public repositories up to a certain storage quota.

## Usage

### GitHub Codespaces

1. Click "Code" button on the GitHub repository
2. Select "Create codespace on main"
3. Wait for the environment to build

### VS Code Local Dev Containers

1. Clone this repository
2. Open in VS Code
3. Click "Reopen in Container" when prompted

## Repository Structure

- `.devcontainer/devcontainer.json`: main devcontainer definition, features,
  VS Code extensions, and lifecycle commands
- `.devcontainer/Dockerfile`: minimal image customization for Python packages
- `.github/workflows/ci.yml`: CI checks for Dockerfile linting, secret
  scanning, image build, and devcontainer smoke testing

## Using as a Template

### Method 1: GitHub Template

Click "Use this template" button to create a new repository

### Method 2: Copy Configuration

```bash
cp -r .devcontainer /path/to/your/project/
```

### Customization

Edit `.devcontainer/devcontainer.json` to add features or tools:

```json
{
  "features": {
    "ghcr.io/devcontainers/features/java:1": {
      "version": "17"
    }
  }
}
```

To add Docker support when you need it:

```json
{
  "features": {
    "ghcr.io/devcontainers/features/docker-outside-of-docker:1": {}
  }
}
```

If you add new features or packages, keep the CI workflow in sync with any new
tooling expectations you want validated during the container smoke test.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
