# hadolint ignore=DL3007
FROM dast1986/slough-dev-dc-generic-base:1.0.0 AS base

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Switch to root user
USER root

# Install Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_26.x | bash - && \
    apt-get install -y --no-install-recommends nodejs

# Switch back to developer user
USER developer

# Configure Starship
ENV PROMPTNAME="NODEJS"

# Set the default command to bash
CMD ["bash"]