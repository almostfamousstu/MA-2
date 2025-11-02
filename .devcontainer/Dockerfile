# Base image: slim Python + system libs
FROM python:3.11-slim

# Set up a non-root user for devcontainer (Codespaces expects this pattern)
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000

# Install apt deps (curl, make, etc.). Add others here as needed (e.g. libssl-dev).
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl \
        git \
        build-essential \
        ca-certificates \
        vim \
    && rm -rf /var/lib/apt/lists/*

# Create non-root user
RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} \
    && chsh -s /bin/bash ${USERNAME}

# Create workspace dir
WORKDIR /workspace

# Copy requirements first for layer caching
COPY requirements.txt /workspace/requirements.txt

# Install Python deps globally in the image so they’re available even before postCreateCommand
RUN pip install --upgrade pip && \
    pip install --no-cache-dir -r /workspace/requirements.txt

# Copy the rest of the repo into the container
# (Codespaces will volume mount over this, but it's useful for local `docker build` testing)
COPY . /workspace

# Default env placeholders (non-secret). Real secrets go in .env at runtime.
ENV APP_ENV=dev \
    LOG_LEVEL=INFO \
    PYTHONUNBUFFERED=1

# Switch to non-root
USER ${USERNAME}

# Default command. In Codespaces this is overridden by the workspace shell, but for local `docker run` it's nice.
CMD [ "bash" ]

# --- Add VS Code Server (code-server) into the devcontainer image ---
# If you prefer pinning, set VERSION and verify SHA.
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
      wget xz-utils ca-certificates \
  && rm -rf /var/lib/apt/lists/*

# Install code-server (runs VS Code in the browser)
# For air-gapped, mirror this binary internally and replace URL.
ENV CODE_SERVER_VERSION=4.92.2
RUN wget -O /tmp/code-server.tar.xz "https://github.com/coder/code-server/releases/download/v${CODE_SERVER_VERSION}/code-server-${CODE_SERVER_VERSION}-linux-amd64.tar.xz" \
  && mkdir -p /opt/code-server \
  && tar -xJf /tmp/code-server.tar.xz -C /opt/code-server --strip-components=1 \
  && ln -s /opt/code-server/bin/code-server /usr/local/bin/code-server \
  && rm -f /tmp/code-server.tar.xz

# Create a place for config/state (owned by non-root user)
RUN mkdir -p /home/vscode/.local/share/code-server /home/vscode/.config/code-server \
  && chown -R vscode:vscode /home/vscode/.local /home/vscode/.config

# Default port for code-server inside container
EXPOSE 8080

