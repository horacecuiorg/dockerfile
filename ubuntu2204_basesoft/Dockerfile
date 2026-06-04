FROM ubuntu:22.04
LABEL org.opencontainers.image.source https://github.com/horacecuiorg/dockerfile
LABEL version="20260604-v1.0.2"
ENV DEBIAN_FRONTEND=noninteractive
ARG TARGETARCH  # amd64 and  arm64

# 一键式安装系统软件与仓库源，确保 UnionFS 单层化并进行极致清理
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        ca-certificates curl wget gnupg lsb-release \
        git unzip zip tar vim nano htop iproute2 iputils-ping net-tools \
        xz-utils openssh-client openssh-server \
        sudo bash tmux netcat-openbsd socat \
        build-essential software-properties-common \
        python3 python3-pip python3-venv && \
    # 1. 配置 Docker CLI 源
    mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list && \
    # 2. 配置 Node.js LTS 源
    curl -fsSL https://deb.nodesource.com/setup_lts.x | bash - && \
    # 3. 安装依赖包
    apt-get update && \
    apt-get install -y --no-install-recommends docker-ce-cli nodejs && \
    # 4. 升级 npm 并清理缓存
    npm install -g npm && \
    # 5. 下载并解压 cloudflared 客户端
    case "$TARGETARCH" in \
      amd64) ARCH="amd64" ;; \
      arm64) ARCH="arm64" ;; \
      *) echo "Unsupported architecture: $TARGETARCH" && exit 1 ;; \
    esac && \
    curl -fsSL -o /usr/local/bin/cloudflared "https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-${ARCH}" && \
    chmod +x /usr/local/bin/cloudflared && \
    cloudflared --version && \
    # 6. 系统收尾清理工作
    mkdir -p /var/run/sshd && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/.npm


# 默认进入 shell
CMD ["/bin/bash"]
# cyhfch/ubuntu2204:basesoft
