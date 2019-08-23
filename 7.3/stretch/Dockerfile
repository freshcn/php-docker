FROM php:7.3.8-stretch

RUN echo "deb http://mirrors.aliyun.com/debian/ stretch main non-free contrib\ndeb-src http://mirrors.aliyun.com/debian/ stretch main non-free contrib\ndeb http://mirrors.aliyun.com/debian-security stretch/updates main\ndeb-src http://mirrors.aliyun.com/debian-security stretch/updates main\ndeb http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib\ndeb-src http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib\ndeb http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib\ndeb-src http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib" > /etc/apt/sources.list && rm -f /etc/apt/sources.list.d/*

RUN apt-get update

WORKDIR /var/www/html/

RUN cd /tmp \ 
    && apt-get install -y libzip-dev librdkafka-dev \
    && curl -fsL https://github.com/websupport-sk/pecl-memcache/archive/4.0.4.tar.gz -o memcache.tar.gz \
    && mkdir -p /tmp/memcache \
    && tar -zvxf memcache.tar.gz -C /tmp/memcache \
    && docker-php-ext-configure /tmp/memcache/pecl-memcache-4.0.4 \
    && docker-php-ext-install -j$(nproc) pdo_mysql opcache  sysvsem sysvmsg sysvshm zip /tmp/memcache/pecl-memcache-4.0.4 pcntl \ 
    && mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" \
    && pecl install swoole-4.4.4 \
    && pecl install redis-4.2.0 \
    && pecl install rdkafka-3.1.2 \
    && docker-php-ext-enable swoole redis rdkafka \
    && rm -fr /tmp/* 
