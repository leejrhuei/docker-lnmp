### 项目结构

```
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
│   ├─ clickhouse           ClickHouse数据目录
│   ├─ mongodb              MongoDB数据目录
│   ├─ mysql                MySQL数据目录
├─ log                      日志目录
│   ├─ nginx                Nginx日志目录
├─ www                      项目目录
│   ├─ go                   Go项目目录
│   ├─ php                  PHP项目目录
├─ docker-compose.yml       容器配置文件
├─ README.md                说明文档
```

### QA

1. 安装Docker客户端？
    - 阿里云后台->工作台->容器镜像服务->镜像工具->镜像加速器

2. 配置镜像加速器？
    - 具体举例见下方
   ```json
   {
      "registry-mirrors": [
         "https://docker.1panel.live",
         "https://docker.anyhub.us.kg",
         "https://docker.awsl9527.cn",
         "https://docker.chenby.cn",
         "https://docker.fxxk.dedyn.io",
         "https://dockerhub.icu",
         "https://dhub.kubesre.xyz"
      ]
   }
   ```

3. docker compose首次构建go时，若提示 `unexpected status code [manifests 1.24.5-bullseye]: 403 Forbidden` ？
    - 可能是限流或网络问题，`docker pull golang:1.24.5-bullseye` 后再重新构建

4. docker compose首次构建php时，若提示 `unexpected status code [manifests 7.3-fpm]: 403 Forbidden` ？
    - 可能是限流或网络问题，`docker pull php:7.3-fpm` 后再重新构建
