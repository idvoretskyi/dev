# Development Environment Repository

[![CI](https://github.com/idvoretskyi/dev/actions/workflows/ci.yml/badge.svg)](https://github.com/idvoretskyi/dev/actions/workflows/ci.yml)
[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/idvoretskyi/dev)

This repository serves as an optimized GitHub Codespaces template for general development workflows.

## Features

- Base image: Ubuntu 24.04
- Essential development tools:
  - Python 3.12 with pip
  - Node.js LTS with npm
  - Git (OS-provided)
  - Docker-in-Docker with Docker Compose v2
  - GitHub CLI
  - Claude Code CLI
  - Zsh with common utilities
  - Build essentials (gcc, make, etc.)
- VS Code extensions:
  - Claude Dev (Anthropic)
  - Python language support
  - Docker support
  - GitHub Copilot
  - YAML support

## Performance Optimizations

The devcontainer balances speed with operability:

- Includes essential tools: Python, Node.js, Docker, Git, GitHub CLI, Claude Code CLI
- Disabled package upgrades during build
- Removed heavy features (kubectl, helm, minikube, sshd)
- Uses OS-provided Git for faster builds
- Installs Claude Code CLI via npm in postCreateCommand
- Core VS Code extensions only

Estimated startup time: 2-3 minutes

## Usage

### GitHub Codespaces
1. Click "Code" button on the GitHub repository
2. Select "Create codespace on main"
3. Wait for the environment to build

### VS Code Local Dev Containers
1. Clone this repository
2. Open in VS Code
3. Click "Reopen in Container" when prompted

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

To add heavy tools like Kubernetes:

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

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
