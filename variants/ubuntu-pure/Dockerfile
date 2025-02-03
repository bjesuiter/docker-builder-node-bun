FROM ubuntu:24.04

# Set environment
ENV NODE_VERSION=22.x
ENV HOME=/root
SHELL ["/bin/bash", "-c"]
WORKDIR /root

# Update the package repository and install nodejs
RUN apt-get update && \
    apt-get install -y curl gnupg2 && \
    curl -fsSL https://deb.nodesource.com/setup_$NODE_VERSION | bash - && \
    apt-get install -y nodejs

# Verify node installation
RUN node -v && npm -v

# install bun
RUN apt-get install -y unzip && \
    curl -fsSL https://bun.sh/install | bash

# make sure the bun paths are available without sourcing the bashrc (since gitlab probably won't do that when using the image to run builds)
ENV BUN_INSTALL="$HOME/.bun"
ENV PATH="$BUN_INSTALL/bin:$PATH"

# Verify bun installation - does not work with QUEMU amd64 build on arm64 mac
# RUN bun -v

# Install more dependencies needed for building maya in gitlab 
RUN apt-get install -y git

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir /workdir
WORKDIR /workdir

# Set the default command to run when starting the container
CMD [ "bash" ]