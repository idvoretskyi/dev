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
  - Docker-in-Docker with Docker Compose v2
  - GitHub CLI
  - Claude Code CLI installed in `postCreateCommand`
  - Zsh and common shell utilities
  - Build essentials (`gcc`, `make`, and related packages)
  - SSH daemon support via the devcontainer feature
- VS Code extensions:
  - Anthropic Claude Code
  - Python and Pylance
  - Docker
  - GitHub Copilot and Copilot Chat
  - YAML

## Performance Optimizations

The devcontainer balances speed with operability:

- Keeps the image focused on core development tooling
- Avoids full package upgrades during image build to reduce rebuild time
- Uses the devcontainer feature set for Node.js, Git, Docker, GitHub CLI, and SSH
- Uses Ubuntu's packaged Python runtime for a faster base setup
- Installs Claude Code CLI in `postCreateCommand` instead of baking it into the image
- Limits editor customizations to a small, broadly useful extension set

Estimated startup time: 2-3 minutes, depending on feature downloads and
network speed.

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
  VS Code extensions, and post-create validation command
- `.devcontainer/Dockerfile`: minimal image customization for extra packages
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
  },
  "postCreateCommand": "pip install -r requirements.txt"
}
```

To add heavier tooling when you actually need it:

```json
{
  "features": {
    "ghcr.io/devcontainers/features/kubectl-helm-minikube:1": {
      "version": "latest",
      "helm": "latest",
      "minikube": "none"
    }
  }
}
```

If you add new features or packages, keep the CI workflow in sync with any new
tooling expectations you want validated during the container smoke test.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
