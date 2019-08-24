FROM php:7.3.8-stretch

RUN echo "deb http://mirrors.aliyun.com/debian/ stretch main non-free contrib\ndeb-src http://mirrors.aliyun.com/debian/ stretch main non-free contrib\ndeb http://mirrors.aliyun.com/debian-security stretch/updates main\ndeb-src http://mirrors.aliyun.com/debian-security stretch/updates main\ndeb http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib\ndeb-src http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib\ndeb http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib\ndeb-src http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib" > /etc/apt/sources.list && rm -f /etc/apt/sources.list.d/*

RUN apt-get update

WORKDIR /var/www/html/

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --install-dir=/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

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
