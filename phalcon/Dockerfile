# ベースイメージ：これを元に作成する
FROM php:7.3-fpm-buster

# 環境変数
ENV TIMEZONE=Asia/Tokyo
ENV PHALCON_VERSION=3.4.5

# 一時変数
ARG PHP_INI_DIR=/usr/local/etc/php

RUN set -ex \
  && apt-get update \
  && apt-get install -y \
    git \
    libicu-dev \
  && docker-php-ext-install intl pdo_mysql sockets \
  && apt-get clean
RUN set -ex && \
    mv "${PHP_INI_DIR}/php.ini-production" "${PHP_INI_DIR}/php.ini" && \
    curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz && \
    tar xzf v${PHALCON_VERSION}.tar.gz && cd cphalcon-${PHALCON_VERSION}/build && ./install && \
    echo "extension=phalcon.so" > ${PHP_INI_DIR}/conf.d/phalcon.ini && \
    cd ../.. && rm -rf v${PHALCON_VERSION}.tar.gz cphalcon-${PHALCON_VERSION}

COPY php.ini /usr/local/etc/php/

# composer/box
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && curl -LSs https://box-project.github.io/box2/installer.php | php \
    && mv box.phar /usr/local/bin/box \
    # phalcon
    && git clone https://github.com/phalcon/phalcon-devtools.git \
    && cd phalcon-devtools \
    && git checkout 3.0.x \
    && box build -v \
    && mv phalcon.phar /usr/local/bin/phalcon \
    && cd .. \
    && rm -rf phalcon-devtools \
    && mkdir /var/www/html/cache