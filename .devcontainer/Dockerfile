FROM mcr.microsoft.com/devcontainers/base:ubuntu26.04

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
    rm -rf /var/lib/apt/lists/*; \
    # Remove Pebble, an unused ACME/TLS test server baked into the base
    # image. It is not owned by apt, has no systemd unit, and is never run,
    # yet its outdated Go runtime (golang.org/x/net, x/sys, stdlib) is the
    # sole source of all fixable HIGH CVEs in the image.
    rm -rf /usr/bin/pebble /var/lib/pebble
