FROM php:7.3.2-cli-alpine3.9

ARG env

RUN sed -i 's/dl-cdn.alpinelinux.org/mirror.tuna.tsinghua.edu.cn/g' /etc/apk/repositories \
    && apk update \
    && apk add composer \
    && apk add --virtual buildDeps autoconf gcc g++ make \ 
    && curl -fSL 'https://github.com/phpredis/phpredis/archive/4.2.0.tar.gz' -o redis-4.2.0.tar.gz \
    && mkdir -p /tmp/redis \
    && tar -zvxf redis-4.2.0.tar.gz -C /tmp/redis --strip-components=1\
    && rm redis-4.2.0.tar.gz \
    && docker-php-ext-configure /tmp/redis \
    && docker-php-ext-install -j$(nproc) pdo_mysql opcache /tmp/redis sysvsem sysvmsg sysvshm \
    && rm -fr /tmp/redis \
    && mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" \
    && apk del buildDeps
