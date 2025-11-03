# Development Environment Repository

[![CI](https://github.com/idvoretskyi/dev/actions/workflows/ci.yml/badge.svg)](https://github.com/idvoretskyi/dev/actions/workflows/ci.yml)
[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/idvoretskyi/dev)

This repository serves as an optimized GitHub Codespaces template for general development workflows.

## Features

- Base image: Ubuntu 24.04
- Essential development tools:
  - Git (OS-provided)
  - Node.js LTS
  - Docker-in-Docker
  - GitHub CLI
  - Zsh with common utilities
- VS Code extensions:
  - Docker support
  - GitHub Copilot
  - YAML support

## Performance Optimizations

The devcontainer is optimized for fast startup:

- Disabled package upgrades during build
- Removed heavy features (kubectl, helm, minikube, sshd)
- Removed unnecessary Docker packages
- Minimal postCreateCommand
- Essential VS Code extensions only

Estimated startup time: 1-2 minutes

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

Edit `.devcontainer/devcontainer.json` to add features:

```json
{
  "features": {
    "ghcr.io/devcontainers/features/python:1": {
      "version": "3.11"
    }
  },
  "postCreateCommand": "pip install -r requirements.txt"
}
```

## Optional Tools

Install additional tools as needed:
```bash
bash .devcontainer/setup-optional-tools.sh
```

Edit the script to customize which tools to install.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
