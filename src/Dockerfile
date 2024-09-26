# hadolint ignore=DL3007
FROM dast1986/slough-dev-dc-generic-base:latest AS base

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Switch to root user
USER root

# Install Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - && \
    apt-get install -y --no-install-recommends nodejs=22.9.0-1nodesource1

# Switch back to developer user
USER developer

# Configure Starship
ENV PROMPTNAME="NODEJS"

# Set the default command to bash
CMD ["bash"]