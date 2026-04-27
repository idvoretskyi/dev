FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# hadolint ignore=DL3008
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    python3 \
    python3-venv \
    python3-pip \
    sudo \
    linux-libc-dev \
    vim-tiny \
    && apt-get -y upgrade gnupg gpg gpg-agent gpgsm gpgconf keyboxd \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
