FROM jenkins/jenkins:lts-jdk17

USER root

# Install basic dependencies
RUN apt-get update && apt-get install -y \
    lsb-release \
    curl \
    wget \
    build-essential

# Install Docker CLI
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg

RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list

RUN apt-get update && apt-get install -y docker-ce-cli

# Install Go
RUN wget -q https://go.dev/dl/go1.21.0.linux-amd64.tar.gz && \
    tar -C /usr/local -xzf go1.21.0.linux-amd64.tar.gz && \
    rm go1.21.0.linux-amd64.tar.gz

# Install Node.js
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs

# Set up Go environment
ENV PATH="/usr/local/go/bin:${PATH}"
ENV GOPATH="/var/jenkins_home/go"
ENV GOCACHE="/var/jenkins_home/.gocache"

# Install go-junit-report
RUN go install github.com/jstemmer/go-junit-report@latest

# Ensure jenkins user owns the Go directories
RUN mkdir -p /var/jenkins_home/go /var/jenkins_home/.gocache && \
    chown -R jenkins:jenkins /var/jenkins_home/go /var/jenkins_home/.gocache

USER jenkins