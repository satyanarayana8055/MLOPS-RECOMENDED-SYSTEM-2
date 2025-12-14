# Jenkins LTS base image
FROM jenkins/jenkins:lts

# Switch to root for installation
USER root

# Install system dependencies, Docker, and Python
RUN apt-get update -y && \
    apt-get install -y \
        ca-certificates \
        curl \
        gnupg \
        lsb-release \
        python3 \
        python3-pip \
        python3-venv && \
    ln -sf /usr/bin/python3 /usr/bin/python && \
    curl -fsSL https://get.docker.com | sh && \
    groupadd -f docker && \
    usermod -aG docker jenkins && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# (Optional) Docker data directory
RUN mkdir -p /var/lib/docker
VOLUME /var/lib/docker

# Switch back to Jenkins user
USER jenkins
