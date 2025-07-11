FROM golang:1.24.5-bullseye

ENV CGO_ENABLED=0 \
    GO111MODULE=on \
    GOPROXY=https://goproxy.cn,direct

RUN apt-get update && apt-get install -y --no-install-recommends \
    procps \
    supervisor

# 清理不需要的文件
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# 启用supervisor(-n前台运行，防止docker自动退出)
CMD ["sh", "-c", "/usr/bin/supervisord -n -c /etc/supervisor/supervisord.conf"]
