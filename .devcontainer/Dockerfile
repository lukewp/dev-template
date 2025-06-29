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
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Optional: Set default shell to bash or zsh
SHELL ["/bin/bash", "-c"]

# Optional: Install Python packages
RUN pip3 install --no-cache-dir --upgrade pip setuptools

# Optional: Create non-root user (works with devcontainers)
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
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
