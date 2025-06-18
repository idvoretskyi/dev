FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# Install essential packages, Claude Code, and set up directories
# hadolint ignore=DL3008
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
        curl \
        wget \
        jq \
        build-essential \
        openssh-server \
        nodejs \
        npm \
        tcl \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/* \
    && npm install -g @anthropic-ai/claude-code@latest \
    && mkdir -p /run/sshd \
    && mkdir -p /home/vscode/.ssh \
    && chown -R vscode:vscode /home/vscode/.ssh \
    && chmod 700 /home/vscode/.ssh
