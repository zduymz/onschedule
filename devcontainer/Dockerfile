FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# Install base system packages
RUN apt-get update && apt-get install -y \
    curl \
    unzip \
    gpg \
    git \
    ca-certificates \
    software-properties-common \
    apt-transport-https \
    lsb-release \
    && rm -rf /var/lib/apt/lists/*

# Install Terraform CLI via HashiCorp APT repository
RUN curl -fsSL https://apt.releases.hashicorp.com/gpg | gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" > /etc/apt/sources.list.d/hashicorp.list \
    && apt-get update \
    && apt-get install -y terraform \
    && rm -rf /var/lib/apt/lists/*

# Install GitHub CLI via official APT repository
RUN curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" > /etc/apt/sources.list.d/github-cli.list \
    && apt-get update \
    && apt-get install -y gh \
    && rm -rf /var/lib/apt/lists/*

# Install AWS CLI v2
RUN ARCH=$(uname -m) && \
    if [ "$ARCH" = "x86_64" ]; then \
        curl -fsSL "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"; \
    elif [ "$ARCH" = "aarch64" ]; then \
        curl -fsSL "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip"; \
    fi && \
    unzip -q awscliv2.zip && \
    ./aws/install && \
    rm -rf awscliv2.zip aws

# Install uv via official installer
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.local/bin:${PATH}"

# Install Claude Code standalone binary
RUN curl -fsSL https://claude.ai/install.sh | sh
ENV PATH="/root/.claude/local/bin:${PATH}"

# Set default working directory
WORKDIR /workspace

# Set default shell to bash
SHELL ["/bin/bash", "-c"]

CMD ["/bin/bash"]
