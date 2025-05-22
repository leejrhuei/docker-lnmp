FROM php:7.3-fpm

# 系统依赖
# 1. imagemagick：pecl扩展-imagick所需
# 2. git/zip/unzip/libzip-dev/supervisor：项目所需
RUN apt-get update && apt-get install -y \
    libssl-dev \
    libcurl4-openssl-dev \
    libz-dev \
    libxml2-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libmagickwand-dev \
    libyaml-dev \
    libmcrypt-dev \
    re2c \
    wget \
    imagemagick \
    git \
    zip \
    unzip \
    libzip-dev \
    supervisor

# 核心扩展
RUN docker-php-ext-install pdo_mysql bcmath gd zip pcntl

# pecl扩展
RUN pecl install redis-5.3.4 \
    && pecl install apcu-5.1.20 \
    && pecl install igbinary-3.2.2 \
    && pecl install imagick-3.4.4 \
    && pecl install mongodb-1.9.1 \
    && pecl install yaml-2.2.1 \
    && pecl install mcrypt-1.0.4

# 自编译扩展-swoole
RUN cd /usr/local/src && wget https://github.com/swoole/swoole-src/archive/refs/tags/v4.6.6.tar.gz \
    && tar zxf v4.6.6.tar.gz && rm -rf v4.6.6.tar.gz \
    && ( \
        cd swoole-src-4.6.6 \
        && phpize && ./configure --with-php-config=php-config --enable-openssl \
        && make && make install \
    ) \
    && rm -rf swoole-src-4.6.6

# 自编译扩展-phalcon
RUN cd /usr/local/src && wget https://github.com/phalcon/cphalcon/archive/refs/tags/v3.4.5.tar.gz \
    && tar -zxvf v3.4.5.tar.gz && rm -rf v3.4.5.tar.gz \
    && ( \
        cd cphalcon-3.4.5/build/php7/64bits \
        && phpize && ./configure --with-php-config=php-config \
        && make && make install \
    ) \
    && rm -rf cphalcon-3.4.5

# 启用扩展
RUN docker-php-ext-enable redis apcu igbinary imagick mongodb yaml mcrypt swoole phalcon

# 配置时区和扩展的特殊设置
RUN echo "date.timezone = \"Asia/Shanghai\"" > /usr/local/etc/php/conf.d/timezone.ini && \
    echo "apc.enable_cli = On" > /usr/local/etc/php/conf.d/apcu.ini && \
    echo "apc.use_request_time = Off" >> /usr/local/etc/php/conf.d/apcu.ini && \
    echo "apc.shm_size = 1024M" >> /usr/local/etc/php/conf.d/apcu.ini && \
    echo "swoole.use_shortname = Off" > /usr/local/etc/php/conf.d/swoole.ini

# 安装composer依赖管理工具
RUN curl -sS https://install.phpcomposer.com/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
    && composer self-update --clean-backups

# 安装node12.x和yarn1.21.1(bm1项目所需)
RUN curl -fsSL https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g yarn@1.21.1

# 清理不需要的文件
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# 启用supervisor(-n前台运行，防止docker自动退出)
CMD ["sh", "-c", "/usr/bin/supervisord -n -c /etc/supervisor/supervisord.conf"]
