### 项目结构

```text
docker-lnmp
├─ build                    构建目录
│   ├─ go                   Go构建目录
│       ├─ Dockerfile       Go构建文件
│   ├─ php                  PHP构建目录
│       ├─ Dockerfile       PHP构建文件
├─ conf                     配置目录
│   ├─ mysql                MySQL配置目录
│       ├─ conf.d           MySQL额外配置目录
│   ├─ nginx                Nginx配置目录
│       ├─ conf.d           Nginx项目配置目录
│   ├─ php                  PHP配置目录
│       ├─ php.ini          PHP配置文件
│   ├─ supervisor           Supervisor配置目录
│       ├─ conf.d           Supervisor项目配置目录
│           ├─ go           Supervisor-Go项目配置目录
│           ├─ php          Supervisor-PHP项目配置目录
├─ data                     数据目录
├─ log                      日志目录
├─ www                      项目目录
│   ├─ fm                   前端项目目录
│   ├─ go                   Go项目目录
│   ├─ php                  PHP项目目录
├─ docker-compose.yml       容器配置文件
├─ README.md                说明文档
```

### QA

1. Mac 安装/升级 Docker 客户端？

```text
对于10.10.3以下的用户 推荐使用Docker Toolbox
Mac安装文件：http://mirrors.aliyun.com/docker-toolbox/mac/docker-toolbox/

对于10.10.3以上的用户 推荐使用Docker for Mac
Mac安装文件：http://mirrors.aliyun.com/docker-toolbox/mac/docker-for-mac/
```

2. 配置镜像源？

```json
{
  "registry-mirrors": [
    "https://2a6bf1988cb6428c877f723ec7530dbc.mirror.swr.myhuaweicloud.com",
    "https://docker.1ms.run",
    "https://proxy.vvvv.ee",
    "https://wget.la",
    "https://dockerproxy.net"
  ]
}
```
