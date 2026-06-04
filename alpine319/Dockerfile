FROM node:23.11.1-alpine
LABEL version="20260604-v1.0.1"

ENV DEBIAN_FRONTEND=noninteractive
ENV PATH=/usr/local/bin:$PATH
ARG TARGETARCH  # amd64 or arm64 

# 安装必要软件
RUN apk update && apk add --no-cache \
    bash \
    curl \
    wget \
    vim \
    nano \
    git \
    openssh \
    sudo \
    iputils \
    xz \
    ca-certificates \
    shadow

# cloudflared 
RUN case "$TARGETARCH" in \
      amd64) ARCH="amd64" ;; \
      arm64) ARCH="arm64" ;; \
      *) echo "Unsupported architecture: $TARGETARCH" && exit 1 ;; \
    esac && \
    curl -fsSL -o /usr/local/bin/cloudflared \
      "https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-${ARCH}" && \
    chmod +x /usr/local/bin/cloudflared && \
    cloudflared --version

# 创建 alpine 用户并赋予 sudo 权限
RUN useradd -m -s /bin/sh alpine && \
    echo "alpine:alpine" | chpasswd && \
    adduser alpine wheel && \
    echo '%wheel ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/wheel && \
    chmod 0440 /etc/sudoers.d/wheel

# 设置工作目录
WORKDIR /home/alpine

# 设置默认用户
USER alpine

CMD ["/bin/sh"]
