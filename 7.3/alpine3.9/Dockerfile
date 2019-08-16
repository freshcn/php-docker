FROM php:7.3.2-cli-alpine3.9

RUN sed -i 's/dl-cdn.alpinelinux.org/mirror.tuna.tsinghua.edu.cn/g' /etc/apk/repositories \
    && apk update \
    && apk add composer zlib zlib-dev \
    && apk add --virtual buildDeps autoconf gcc g++ make\ 
    && cd /tmp \ 
    && curl -fSL 'https://github.com/phpredis/phpredis/archive/4.2.0.tar.gz' -o redis-4.2.0.tar.gz \
    && mkdir -p /tmp/redis \
    && tar -zvxf redis-4.2.0.tar.gz -C /tmp/redis --strip-components=1\
    && rm redis-4.2.0.tar.gz \
    && docker-php-ext-configure /tmp/redis \
    && curl -fsL https://github.com/websupport-sk/pecl-memcache/archive/4.0.4.tar.gz -o memcache.tar.gz \
    && mkdir -p /tmp/memcache \
    && tar -zvxf memcache.tar.gz -C /tmp/memcache \
    && docker-php-ext-configure /tmp/memcache/pecl-memcache-4.0.4 \
    && apk add libzip-dev \
    && docker-php-ext-install -j$(nproc) pdo_mysql opcache /tmp/redis sysvsem sysvmsg sysvshm zip /tmp/memcache/pecl-memcache-4.0.4 pcntl \ 
    && rm -fr /tmp/redis /tmp/memcache/pecl-memcache-4.0.4 \
    && mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" \
    && rm -fr /tmp/* \
    && apk del buildDeps
