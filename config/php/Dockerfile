ARG PHP_VERSION
FROM php:${PHP_VERSION}
MAINTAINER Fate bboyfate@163.com

#ENV PHP_VARS /usr/local/etc/php/php.ini
#ENV FPM_CONF /usr/local/etc/php-fpm.d/www.conf
#ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php

ARG ALPINE_REPOSITORIES

RUN if [ "${ALPINE_REPOSITORIES}" != "" ]; then \
      sed -i "s/dl-cdn.alpinelinux.org/${ALPINE_REPOSITORIES}/g" /etc/apk/repositories; \
    fi

RUN addgroup -S www \
  && adduser -D -S -h /var/www -s /sbin/nologin -G www www \
  && apk add --no-cache --virtual .build-deps \
      autoconf \
      gcc \
      vim \
      git \
      libc-dev \
      make \
      openssl-dev \
      pcre-dev \
      zlib-dev \
      linux-headers \
      curl \
      gnupg \
      libxslt-dev \
      gd-dev \
      geoip-dev \
      perl-dev

RUN apk update && \
    apk add --no-cache bash \
    openssh-client \
    wget \
    supervisor \
    curl \
    libcurl \
    python \
    python-dev \
    py-pip \
    augeas-dev \
    openssl-dev \
    ca-certificates \
    dialog \
    autoconf \
    make \
    gcc \
    musl-dev \
    linux-headers \
    libmcrypt-dev \
    libpng-dev \
    icu-dev \
    libpq \
    libxslt-dev \
    libffi-dev \
    freetype-dev \
    sqlite-dev \
    libjpeg-turbo-dev

# 必须这样装mcrypt
RUN pecl install mcrypt-1.0.3 && \
    pecl install redis

# 跑GD要配置下
RUN docker-php-ext-configure gd \
      --enable-gd \
      --with-freetype-dir=/usr/include/freetype2 \
      --with-png \
      --with-jpeg && \
  docker-php-ext-install gd

RUN docker-php-ext-enable redis.so && \
  docker-php-ext-enable mcrypt.so && \
  docker-php-ext-install pdo_mysql && \
  docker-php-ext-install mysqli && \
  docker-php-ext-install bcmath && \
  docker-php-source delete

# 安装 composer
RUN curl --silent --show-error https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
  && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/


## 优化 php-fpm 配置
#RUN echo "cgi.fix_pathinfo=0" > ${PHP_VARS} &&\
#    echo "upload_max_filesize = 100M"  >> ${PHP_VARS} &&\
#    echo "post_max_size = 100M"  >> ${PHP_VARS} &&\
#    echo "variables_order = \"EGPCS\""  >> ${PHP_VARS} && \
#    echo "memory_limit = -1"  >> ${PHP_VARS} && \
#    echo "date.timezone =  Asia/Shanghai"  >> ${PHP_VARS}
#
#RUN sed -i \
#    -e "s/;catch_workers_output\s*=\s*yes/catch_workers_output = yes/g" \
#    -e "s/pm.max_children = 5/pm.max_children = 4/g" \
#    -e "s/pm.start_servers = 2/pm.start_servers = 3/g" \
#    -e "s/pm.min_spare_servers = 1/pm.min_spare_servers = 2/g" \
#    -e "s/pm.max_spare_servers = 3/pm.max_spare_servers = 4/g" \
#    -e "s/;pm.max_requests = 500/pm.max_requests = 200/g" \
#    -e "s/user = www-data/user = www/g" \
#    -e "s/group = www-data/group = www/g" \
#    -e "s/;listen.mode = 0660/listen.mode = 0666/g" \
#    -e "s/;listen.owner = www-data/listen.owner = www/g" \
#    -e "s/;listen.group = www-data/listen.group = www/g" \
#    -e "s/listen = 127.0.0.1:9000/listen = \/dev\/shm\/php-fpm.sock/g" \
#    -e "s/^;clear_env = no$/clear_env = no/" \
##    ${FPM_CONF}

WORKDIR /var/www
