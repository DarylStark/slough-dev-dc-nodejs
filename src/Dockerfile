# Use a minimal base image
FROM slough-dev-dc-generic-base:0.0.3 AS base

# Install Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_22.x | sudo bash - && \
    sudo apt-get install -y nodejs

# Configure Starship
ENV PROMPTNAME="NODEJS"

# Set the default command to bash
CMD ["bash"]