# Use the GitHub Actions runner as the base image
FROM ghcr.io/actions/actions-runner:2.326.0

# Temporarily switch to root user for installations
USER root

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV DOCKER_CONFIG=/kaniko/.docker

# Install system dependencies and tools
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    wget \
    git \
    jq \
    tree \
    unzip \
    iputils-ping \
    telnet \
    sudo \
    vim \
    dnsutils \
    traceroute \
    net-tools \
    tcpdump \
    tinyproxy \
    ca-certificates \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Copy the latest Kaniko executor and configure it
COPY --from=gcr.io/kaniko-project/executor:latest /kaniko/executor /kaniko/executor
COPY --from=gcr.io/kaniko-project/executor:latest /kaniko/ssl/certs/ /kaniko/ssl/certs/

# Create Maven local repository directory
RUN mkdir -p /root/.m2

# Set up required directories
RUN mkdir -p /kaniko/.docker \
    && mkdir -p /home/runner/_work \
    && mkdir -p /workspace \
    && mkdir -p /kaniko/warmer \
    && mkdir -p /cache \
    && mkdir -p /scratch

# Set working directory
WORKDIR /home/runner

# Switch back to runner user
USER runner

# Set the entrypoint
ENTRYPOINT ["/home/runner/run.sh"]