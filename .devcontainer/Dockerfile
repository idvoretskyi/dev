FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# Install essential packages
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
    && rm -rf /var/lib/apt/lists/*

# Set up SSH directory
RUN mkdir -p /run/sshd

# Create the directory structure for the SSH keys
RUN mkdir -p /home/vscode/.ssh && \
    chown -R vscode:vscode /home/vscode/.ssh && \
    chmod 700 /home/vscode/.ssh
