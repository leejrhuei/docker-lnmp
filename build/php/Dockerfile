FROM php:7.2-fpm

LABEL maintainer="leejrhuei<leejrhuei@126.com>"

# set timezone
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
  && echo 'Asia/Shanghai' > /etc/timezone

RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list \
    && sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list

# always run apt update when start and after add new source list, then clean up at end.
# init
RUN apt-get update -yqq && \
    apt-get install -y --no-install-recommends \
    autoconf \
    automake \
    curl \
    git \
    keychain \
    libbz2-dev \
    libevent-dev \
    libfreetype6-dev \
    libgeoip-dev \
    libgmp3-dev \
    libicu-dev \
    libjpeg-dev \
    libjpeg62-turbo-dev \
    libldap2-dev \
    imagemagick \
    libmagick++-dev \
    libmaxminddb0 \
    libmaxminddb-dev \
    mmdb-bin \
    libmcrypt-dev \
    libmemcached-dev \
    libpng-dev \
    libpq-dev \
    libsqlite3-0 \
    libsqlite3-dev \
    libssl-dev \
    libtidy-dev \
    libtool \
    libxml2 \
    libxml2-dev \
    libyaml-dev \
    libz-dev \
    libzip-dev \
    locales \
    procps \
    tmux \
    unzip \
    vim \
    wget \
    zip \
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

# 替换VIM鼠标状态为非可视状态
#RUN sed -i 's/mouse\=a/mouse=v/g' /usr/share/vim/vim80/defaults.vim

RUN echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen && locale-gen

ENV LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8

# install hiredis
RUN wget --no-check-certificate https://gitee.com/oschina/hiredis/repository/archive/v0.13.3.zip -O hiredis.zip \
    && mkdir -p hiredis \
    && unzip hiredis.zip \
    && rm hiredis.zip \
    && ( \
        cd hiredis \
        && make -j$(nproc) \
        && make install \
        && ldconfig \
    ) \
    && rm -r hiredis

# install php extentions
RUN pecl channel-update pecl.php.net \
    && docker-php-ext-configure gd \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2 \
    --with-zlib-dir=/usr \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install -j$(nproc) dba bz2 calendar ldap zip json tidy xml xmlrpc wddx pdo_sqlite bcmath mysqli pdo_mysql gd sockets pcntl intl soap gmp mbstring exif bcmath gettext shmop sysvmsg sysvsem sysvshm \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && pecl install event \
    && docker-php-ext-enable event \
    && pecl install grpc \
    && docker-php-ext-enable grpc \
    && pecl install inotify \
    && docker-php-ext-enable inotify \
    && pecl install memcached \
    && docker-php-ext-enable memcached \
    && pecl install mongodb \
    && docker-php-ext-enable mongodb \
    && pecl install msgpack \
    && docker-php-ext-enable msgpack \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && pecl install seaslog \
    && docker-php-ext-enable seaslog \
    && pecl clear-cache

# install geoip extention
RUN wget --no-check-certificate https://pecl.php.net/get/geoip-1.1.1.tgz \
    && tar -zxvf geoip-1.1.1.tgz \
    && rm geoip-1.1.1.tgz \
    && ( \
        cd geoip-1.1.1 \
        && phpize \
        && ./configure \
        && make \
        && make install \
    ) \
    && rm -r geoip-1.1.1 \
    && docker-php-ext-enable geoip

# install imagick extention
RUN wget --no-check-certificate https://pecl.php.net/get/imagick-3.4.4.tgz \
    && tar -zxvf imagick-3.4.4.tgz \
    && rm imagick-3.4.4.tgz \
    && ( \
        cd imagick-3.4.4 \
        && phpize \
        && ./configure \
        && make \
        && make install \
    ) \
    && rm -r imagick-3.4.4 \
    && docker-php-ext-enable imagick

# install maxminddb extention
RUN wget --no-check-certificate https://github.com/maxmind/MaxMind-DB-Reader-php/archive/master.zip -O MaxMind-DB-Reader-php.zip \
    && unzip MaxMind-DB-Reader-php.zip \
    && rm MaxMind-DB-Reader-php.zip \
    && ( \
        cd MaxMind-DB-Reader-php-master/ext \
        && phpize \
        && ./configure \
        && make \
        && make install \
    ) \
    && rm -r MaxMind-DB-Reader-php-master \
    && docker-php-ext-enable maxminddb

# install phalcon extention
RUN wget --no-check-certificate https://gitee.com/mirrors/cphalcon/repository/archive/v3.4.1.zip -O phalcon.zip \
    && unzip phalcon.zip \
    && rm phalcon.zip \
    && ( \
        cd cphalcon/build \
        && ./install \
    ) \
    && rm -r cphalcon \
    && docker-php-ext-enable phalcon

# install swoole extention
RUN wget --no-check-certificate https://gitee.com/swoole/swoole/repository/archive/v4.2.2.zip -O swoole.zip \
    && mkdir -p swoole \
    && unzip swoole.zip \
    && rm swoole.zip \
    && ( \
        cd swoole \
        && phpize \
        && ./configure --enable-async-redis --enable-mysqlnd --enable-openssl \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r swoole \
    && docker-php-ext-enable swoole

# install yaml extention
RUN wget --no-check-certificate https://pecl.php.net/get/yaml-2.0.4.tgz -O yaml.tgz \
    && tar -zxvf yaml.tgz \
    && rm yaml.tgz \
    && ( \
        cd yaml-2.0.4 \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r yaml-2.0.4 \
    && docker-php-ext-enable yaml

# install composer
RUN curl -sS https://install.phpcomposer.com/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer self-update --clean-backups \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com

# 对Seaslog默认存储目录增加www:www owner && 对php默认bash登陆目录赋予www:www owner
RUN useradd www && chown -R www:www /var/log/www && chown -R www:www /var/www/html

EXPOSE 9000
CMD ["php-fpm"]
