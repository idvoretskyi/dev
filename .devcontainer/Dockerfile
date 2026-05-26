FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# hadolint ignore=DL3008
RUN set -eux; \
    export DEBIAN_FRONTEND=noninteractive; \
    apt-get update; \
    apt-get -y dist-upgrade; \
    apt-get -y install --no-install-recommends \
        python3 \
        python3-venv \
        python3-pip \
        sudo; \
    apt-get -y purge linux-libc-dev; \
    apt-get -y autoremove --purge; \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*
