### 目录结构
```
docker-lnmp
├─ build                    构建目录
│  ├─ php                   PHP构建目录
│     ├─ Dockerfile         PHP构建文件
├─ conf                     配置目录
│  ├─ elasticsearch         ElasticSearch配置目录
│     ├─ elasticsearch.yml  ElasticSearch配置文件
│  ├─ mysql                 MySQL配置目录
│     ├─ my.cnf             MySQL配置文件
│  ├─ nginx                 Nginx配置目录
│     ├─ conf.d             Nginx站点配置目录
│        ├─ blog.conf       blog站点配置文件
│        ├─ default.conf    默认站点配置文件
│     ├─ nginx.conf         Nginx通用配置文件
│  ├─ php                   PHP配置目录
│     ├─ php.ini            PHP默认配置文件
│     ├─ php-fpm.conf       PHP-FPM配置文件
│  ├─ redis                 Redis配置目录
│     ├─ redis.conf         Redis配置文件
├─ data                     数据目录
├─ log                      日志目录
├─ www                      项目根目录
│  ├─ index.php             项目根文件
├─ .dockerignore            容器启动忽略文件
├─ docker-compose.yml       容器启动配置文件
├─ README.md                说明文档
```
