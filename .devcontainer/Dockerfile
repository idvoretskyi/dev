FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# Install additional packages if needed
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
        curl \
        wget \
        jq \
        build-essential \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Set up additional configuration if needed
