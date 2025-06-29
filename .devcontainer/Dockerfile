# syntax=docker/dockerfile:1.4

# ✅ Multi-arch compatible base image
FROM mcr.microsoft.com/devcontainers/base:ubuntu

# Metadata
LABEL maintainer="you@example.com"

# Update and install core dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    unzip \
    jq \
    sudo \
    zsh \
    r-base \
    python3 \
    python3-pip \
    python3-venv \
    python3-setuptools \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Optional: Set default shell to bash or zsh
SHELL ["/bin/bash", "-c"]

# Setup Python virtual environment
RUN python3 -m venv /opt/venv \
    && /opt/venv/bin/pip install --no-cache-dir --upgrade pip setuptools

ENV PATH="/opt/venv/bin:$PATH"

# Install Python packages
RUN pip3 install --no-cache-dir --upgrade pip setuptools

# Optional: Create non-root user (works with devcontainers)
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000
# Prevent user creation failure if the vscode group already exists
RUN if ! getent group $USERNAME >/dev/null; then \
      groupadd --gid $USER_GID $USERNAME; \
    fi \
 && if ! id $USERNAME >/dev/null 2>&1; then \
      useradd --uid $USER_UID --gid $USER_GID -m $USERNAME; \
    fi \
 && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/$USERNAME \
 && chmod 0440 /etc/sudoers.d/$USERNAME


# Set work directory
WORKDIR /workspace

# R tidyverse and renv setup:
RUN Rscript -e "install.packages(c('tidyverse', 'renv'), repos='https://cloud.r-project.org/')"

# Set environment variables (example only)
ENV LANG=C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive

# Done
CMD ["bash"]
