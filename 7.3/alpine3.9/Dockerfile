FROM php:7.3.2-cli-alpine3.9

RUN sed -i 's/dl-cdn.alpinelinux.org/mirror.tuna.tsinghua.edu.cn/g' /etc/apk/repositories \
    && apk update \
    && apk add composer zlib zlib-dev \
    && apk add --virtual buildDeps autoconf gcc g++ make 
WORKDIR /var/www/html/
RUN cd /tmp \ 
    && curl -fsL https://github.com/websupport-sk/pecl-memcache/archive/4.0.4.tar.gz -o memcache.tar.gz \
    && mkdir -p /tmp/memcache \
    && tar -zvxf memcache.tar.gz -C /tmp/memcache \
    && docker-php-ext-configure /tmp/memcache/pecl-memcache-4.0.4 \
    && apk add libzip-dev \
    && docker-php-ext-install -j$(nproc) pdo_mysql opcache  sysvsem sysvmsg sysvshm zip /tmp/memcache/pecl-memcache-4.0.4 pcntl \ 
    && mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" \
    && pecl install swoole-4.4.4 \
    && pecl install redis-4.2.0 \
    && docker-php-ext-enable swoole redis \
    && rm -fr /tmp/* 
