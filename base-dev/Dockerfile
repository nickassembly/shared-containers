# Base image — Microsoft's official .NET 10 SDK image (Debian-based)
FROM mcr.microsoft.com/dotnet/sdk:10.0

# System packages and tools
# Combine into a single RUN to minimize layers and image size
RUN apt-get update && apt-get install -y --no-install-recommends \
        curl \
        git \
        nano \
        htop \
        tree \
        jq \
        build-essential \
        sudo \
        unzip \
        ca-certificates \
        gnupg \
        procps \
        iproute2 \
        openssh-client \
    && rm -rf /var/lib/apt/lists/*

# Using NodeSource for current LTS. No nvm needed in a container.
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - \
    && apt-get install -y --no-install-recommends nodejs \
    && npm install -g pnpm

# non-root user for development
RUN adduser --disabled-password --gecos '' devuser \
    && echo "devuser ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
    && mkdir -p /workspace \
    && chown -R devuser:devuser /workspace

# Switch to non-root user 
USER devuser
WORKDIR /home/devuser

# .NET global tools install into ~/.dotnet/tools, must be on PATH
ENV PATH="${PATH}:/home/devuser/.dotnet/tools"

RUN dotnet tool install --global dotnet-ef

WORKDIR /workspace
CMD ["bash"]