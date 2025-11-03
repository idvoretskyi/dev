FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# hadolint ignore=DL3008
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y upgrade --no-install-recommends \
    && apt-get -y install --no-install-recommends \
        curl \
        wget \
        jq \
        build-essential \
        python3 \
        python3-venv \
        python3-pip \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /home/vscode/.ssh \
    && chown -R vscode:vscode /home/vscode \
    && chmod 700 /home/vscode/.ssh
