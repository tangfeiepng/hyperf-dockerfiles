FROM php:7.4.30-bullseye

COPY ./sources.list.stretch /etc/apt/sources.list
WORKDIR /docker

RUN apt-get update && apt-get install -y zip

RUN curl -L -o /tmp/redis.tar.gz https://pecl.php.net/get/redis-5.0.0.tgz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv redis-5.0.0 /usr/src/php/ext/redis

RUN curl -L -o /tmp/swoole.tar.gz https://pecl.php.net/get/swoole-4.6.1.tgz \
    && tar xfz /tmp/swoole.tar.gz \
    && rm -r /tmp/swoole.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv swoole-4.6.1 /usr/src/php/ext/swoole

RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

RUN docker-php-ext-install json redis swoole pcntl && echo "swoole.use_shortname = 'Off'" >> /usr/local/etc/php/conf.d/php.ini

COPY start-container /usr/local/bin/start-container
RUN chmod +x /usr/local/bin/start-container

ENTRYPOINT ["start-container"]


