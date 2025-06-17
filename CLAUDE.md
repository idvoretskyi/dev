# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a generic GitHub Codespace/devcontainer template repository that provides a standardized development environment. The repository focuses on devcontainer configuration rather than specific application code.

## Architecture

- **Base Environment**: Ubuntu 24.04 via Microsoft devcontainer base image
- **Container Configuration**: Uses Docker Compose with devcontainer.json for proper service orchestration
- **Tool Stack**: Pre-configured with Docker-in-Docker, Kubernetes (kubectl, helm, minikube), Git, GitHub CLI, and SSH
- **User Dotfiles**: Integrates with idvoretskyi/dotfiles repository for personalized shell configuration
- **SSH Access**: Pre-configured SSH server with proper directory structure for remote development

## Key Components

- `.devcontainer/devcontainer.json`: Main devcontainer configuration using Docker Compose with features, VS Code extensions, and settings
- `.devcontainer/Dockerfile`: Custom Ubuntu image with essential development packages (curl, wget, jq, build-essential, openssh-server, nodejs, npm)
- `.devcontainer/docker-compose.yml`: Container orchestration with volume mounts and SSH port forwarding

## Common Commands

### Development Environment
The environment includes these pre-configured tools:
- Docker and Docker Compose (via docker-in-docker)
- Kubernetes tools: kubectl, helm, minikube
- GitHub CLI (gh)
- Node.js, npm, and npx
- SSH server (automatically started on port 22)

## VS Code Configuration

Pre-installed extensions:
- Docker support (ms-azuretools.vscode-docker)
- GitHub Copilot and Copilot Chat
- YAML support (redhat.vscode-yaml)  
- Kubernetes tools (ms-kubernetes-tools.vscode-kubernetes-tools)

Editor settings enforce:
- Format on save
- 80/120 character rulers
- Zsh as default terminal
- Trailing whitespace trimming
- Bracket pair colorization

## Environment Customization

To customize this template:
- Modify `.devcontainer/devcontainer.json` for VS Code extensions/settings
- Edit `.devcontainer/Dockerfile` for additional system packages
- Update `.devcontainer/docker-compose.yml` for additional services