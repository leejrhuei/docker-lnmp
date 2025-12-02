FROM golang:1.24.5-bullseye

# 配置环境变量
ENV CGO_ENABLED=0 \
    GO111MODULE=on \
    GOPROXY=https://goproxy.cn,direct

# 安装系统依赖
# 1. 项目所需：git supervisor vim
RUN apt-get update && apt-get install -y --no-install-recommends \
    procps \
    git supervisor vim

# 清理不需要的文件
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# 启用Supervisor(-n前台运行，防止Docker自动退出)
CMD ["sh", "-c", "/usr/bin/supervisord -n -c /etc/supervisor/supervisord.conf"]
