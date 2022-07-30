FROM php:8.1.8-bullseye

COPY ./sources.list.stretch /etc/apt/sources.list
WORKDIR /docker

RUN apt-get update && apt-get install -y zip libssl-dev libgmp-dev

RUN curl -L -o /tmp/msgpack.tar.gz https://pecl.php.net/get/msgpack-2.1.2.tgz \
    && tar xfz /tmp/msgpack.tar.gz \
    && rm -r /tmp/msgpack.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv msgpack-2.1.2 /usr/src/php/ext/msgpack \
    && docker-php-ext-install pcntl msgpack pdo_mysql gmp

RUN curl -L -o /tmp/redis.tar.gz https://pecl.php.net/get/redis-5.3.7.tgz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv redis-5.3.7 /usr/src/php/ext/redis \
    && docker-php-ext-configure redis --enable-redis-msgpack \
    && docker-php-ext-install -j$(nproc) redis

RUN curl -L -o /tmp/swoole.tar.gz https://pecl.php.net/get/swoole-4.8.11.tgz \
    && tar xfz /tmp/swoole.tar.gz \
    && rm -r /tmp/swoole.tar.gz \
    && mv swoole-4.8.11 /usr/src/php/ext/swoole \
    && docker-php-ext-configure swoole --enable-openssl --enable-http2 \
    && docker-php-ext-install -j$(nproc) swoole

RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

RUN echo "swoole.use_shortname = 'Off'" >> /usr/local/etc/php/conf.d/php.ini


#安装ssh服务
RUN apt-get install -y openssh-server && mkdir /var/run/sshd
COPY sshd_config /etc/ssh/sshd_config

COPY start-container /usr/local/bin/start-container
RUN chmod +x /usr/local/bin/start-container

ENTRYPOINT ["start-container"]