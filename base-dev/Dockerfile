FROM mcr.microsoft.com/dotnet/sdk:10.0

RUN apt-get update && apt-get install -y \
    curl git vim htop tree jq build-essential sudo unzip \
    && rm -rf /var/lib/apt/lists/*

# kubectl only — minikube runs on the WSL2 host, not inside the container.
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" \
    && chmod +x kubectl && mv kubectl /usr/local/bin/kubectl

# Non-root user first, then install tools AS that
RUN adduser --disabled-password --gecos '' devuser \
    && echo "devuser ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
    && mkdir -p /workspace /home/devuser/.kube \
    && chown -R devuser:devuser /workspace /home/devuser

# Global pi tooling available in all projects
RUN npm install -g @mariozechner/pi-coding-agent @mariozechner/pi-tui

# Install nvm + Node inside the container
ENV NVM_DIR=/root/.nvm
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash \
    && . "$NVM_DIR/nvm.sh" \
    && nvm install --lts \
    && nvm use --lts \
    && npm install -g pnpm pi-ai pi-tui

USER devuser
WORKDIR /home/devuser

# .NET tools install as devuser
ENV PATH="${PATH}:/home/devuser/.dotnet/tools"
RUN dotnet tool install --global dotnet-ef

WORKDIR /workspace
