# Use a minimal base image
FROM slough-dev-dc-generic-base:0.0.1 AS base

# Install Node.js
RUN sudo apk add libstdc++
RUN wget https://unofficial-builds.nodejs.org/download/release/v21.7.3/node-v21.7.3-linux-x64-musl.tar.gz
RUN sudo mkdir -p /opt/nodejs
RUN sudo tar -zxvf *.tar.gz --directory /opt/nodejs --strip-components=1
RUN rm *.tar.gz
RUN sudo ln -s /opt/nodejs/bin/node /usr/local/bin/node
RUN sudo ln -s /opt/nodejs/bin/npm /usr/local/bin/npm

# Configure Starship
ENV PROMPTNAME="NODEJS"

# Set the default command to bash
CMD ["bash"]