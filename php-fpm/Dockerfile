FROM php:7-fpm

ENV TZ=Asia/Shanghai

COPY sources.list /etc/apt/sources.list

RUN set -xe \
    && buildDeps=" \
        build-essential \
        php5-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
    " \
    && runtimeDeps=" \
        libfreetype6 \
        libjpeg62-turbo \
        libmcrypt4 \
        libpng12-0 \
    " \
    && DEBIAN_FRONTEND=noninteractive \
    && apt-get update \
    && apt-get install -y ${runtimeDeps} ${buildDeps} --no-install-recommends \
    && docker-php-ext-install iconv mcrypt mysqli pdo pdo_mysql zip \
    && docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \

    #&& pecl install -o -f redis \
    #&&  rm -rf /tmp/pear \
    #&&  docker-php-ext-enable redis \

    && apt-get purge -y --auto-remove \
        -o APT::AutoRemove::RecommendsImportant=false \
        -o APT::AutoRemove::SuggestsImportant=false \
        $buildDeps \
    && rm -rf /var/cache/apt/* \
    && rm -rf /var/lib/apt/lists/*

# 由于某些原因，很多网站在中国访问很慢，手动编译
# 编译安装redis扩展
ADD ./phpredis-php7.tar.gz /tmp
WORKDIR /tmp/phpredis-php7
RUN /usr/local/bin/phpize \
&& ./configure --with-php-config=/usr/local/bin/php-config \
&& make \
&& make install