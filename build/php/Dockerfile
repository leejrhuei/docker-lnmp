FROM php:7.3-fpm

# 安装系统依赖
# 1. 官方扩展-gd：libfreetype6-dev libjpeg-dev libpng-dev
# 2. 官方扩展-gmp：libgmp-dev
# 3. 官方扩展-zip：libzip-dev
# 4. PECL扩展-imagick：libmagickwand-dev imagemagick
# 5. PECL扩展-mcrypt：libmcrypt-dev
# 6. PECL扩展-yaml：libyaml-dev
# 7. 项目所需：git supervisor vim
RUN apt-get update && apt-get install -y --no-install-recommends \
    libcurl4-openssl-dev libssl-dev libxml2-dev libz-dev openssh-client procps re2c wget zip unzip \
    libfreetype6-dev libjpeg-dev libpng-dev \
    libgmp-dev \
    libzip-dev \
    libmagickwand-dev imagemagick \
    libmcrypt-dev \
    libyaml-dev \
    git supervisor vim

# 安装官方扩展
RUN docker-php-ext-install bcmath gd gmp pcntl pdo_mysql zip

# 安装PECL扩展
RUN pecl install apcu-5.1.20 \
    && pecl install igbinary-3.2.2 \
    && pecl install imagick-3.4.4 \
    && pecl install mcrypt-1.0.4 \
    && pecl install mongodb-1.9.1 \
    && pecl install redis-5.3.4 \
    && pecl install yaml-2.2.1

# 安装自编译扩展-Phalcon
RUN cd /usr/local/src && wget https://github.com/phalcon/cphalcon/archive/refs/tags/v3.4.5.tar.gz \
    && tar -zxvf v3.4.5.tar.gz && rm -rf v3.4.5.tar.gz \
    && ( \
        cd cphalcon-3.4.5/build/php7/64bits \
        && phpize && ./configure --with-php-config=php-config \
        && make && make install \
    ) \
    && rm -rf cphalcon-3.4.5

# 安装自编译扩展-Swoole
RUN cd /usr/local/src && wget https://github.com/swoole/swoole-src/archive/refs/tags/v4.6.6.tar.gz \
    && tar zxf v4.6.6.tar.gz && rm -rf v4.6.6.tar.gz \
    && ( \
        cd swoole-src-4.6.6 \
        && phpize && ./configure --with-php-config=php-config --enable-openssl \
        && make && make install \
    ) \
    && rm -rf swoole-src-4.6.6

# 启用PECL和自编译扩展
RUN docker-php-ext-enable apcu igbinary imagick mcrypt mongodb phalcon redis swoole yaml

# 配置时区和扩展
RUN echo "date.timezone = \"Asia/Shanghai\"" > /usr/local/etc/php/conf.d/timezone.ini && \
    echo "apc.enable_cli = On" > /usr/local/etc/php/conf.d/apcu.ini && \
    echo "apc.use_request_time = Off" >> /usr/local/etc/php/conf.d/apcu.ini && \
    echo "apc.shm_size = 1024M" >> /usr/local/etc/php/conf.d/apcu.ini && \
    echo "swoole.use_shortname = Off" > /usr/local/etc/php/conf.d/swoole.ini

# 安装Composer依赖管理工具
RUN curl -sS https://getcomposer.org/installer | php -- --version=2.2.6 \
    && mv composer.phar /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer

# 安装Node和Yarn(bm1项目所需)
RUN curl -fsSL https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g yarn@1.21.1

# 清理不需要的文件
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# 启用Supervisor(-n前台运行，防止Docker自动退出)
CMD ["sh", "-c", "/usr/bin/supervisord -n -c /etc/supervisor/supervisord.conf"]
