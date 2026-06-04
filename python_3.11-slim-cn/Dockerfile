# 使用官方 Python 3.11 slim 镜像
FROM python:3.11-slim
LABEL version="20260604-v1.0.2"

# 设置非交互式模式，避免 tzdata 等软件包安装时阻塞
ENV DEBIAN_FRONTEND=noninteractive


# 彻底清理默认源并设置阿里云 APT 源为 Bookworm 版本
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        build-essential \
        curl wget traceroute telnet tcpdump \
        git netcat-openbsd nmap \
        ca-certificates iputils-ping iproute2 dnsutils \
    && apt-get clean && rm -rf /var/lib/apt/lists/* && \
    rm -f /etc/apt/sources.list.d/* && \
    cat <<EOF > /etc/apt/sources.list
deb http://mirrors.aliyun.com/debian bookworm main contrib non-free
deb http://mirrors.aliyun.com/debian bookworm-updates main contrib non-free
deb http://mirrors.aliyun.com/debian-security bookworm-security main contrib non-free
EOF

# 配置 pip 使用阿里云镜像源
RUN python -m pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir --upgrade flask requests python-dotenv httpx fastapi uvicorn aiosqlite python-multipart gunicorn aiohttp debugpy dingtalk-stream && \
    mkdir -p /root/.pip && \
    cat <<EOF > /root/.pip/pip.conf
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
trusted-host = mirrors.aliyun.com
EOF

COPY ./app /app

EXPOSE 8000
EXPOSE 5678
WORKDIR /app
# 可选：设置容器启动命令（例如运行 FastAPI、Django 等）
CMD ["python", "main.py"]
# CMD ["python", "-m", "debugpy", "--listen", "0.0.0.0:5678", "--wait-for-client", "-m", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
