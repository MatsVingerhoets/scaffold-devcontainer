# .devcontainer/Dockerfile
FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive \
    TZ=Etc/UTC \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8

# Base tools, Neovim, Node.js 24, etc.
RUN apt-get update && \
    apt-get install -y \
      sudo \
      git \
      curl \
      ca-certificates \
      build-essential \
      neovim \
      locales \
      ripgrep \
      fd-find \
      bash-completion \
      tmux \
      unzip \
      zip && \
    locale-gen en_US.UTF-8 && \
    ln -s "$(command -v fdfind)" /usr/local/bin/fd && \
    rm -rf /var/lib/apt/lists/*

# Install Node.js 24 via NodeSource
RUN apt-get update && \
    apt-get install -y curl ca-certificates gnupg && \
    curl -fsSL https://deb.nodesource.com/setup_24.x | bash - && \
    apt-get install -y nodejs && \
    rm -rf /var/lib/apt/lists/*

# Optional: enable corepack + Yarn classic
RUN corepack enable && \
    corepack prepare yarn@1.22.22 --activate || true

# Non-root user
ARG USERNAME=dev
ARG USER_UID=1000
ARG USER_GID=1000

RUN groupadd --gid ${USER_GID} ${USERNAME} && \
    useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} && \
    usermod -aG sudo ${USERNAME} && \
    echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/90-${USERNAME} && \
    chmod 0440 /etc/sudoers.d/90-${USERNAME}

USER ${USERNAME}
WORKDIR /workspace

CMD ["bash"]
