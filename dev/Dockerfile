FROM debian:jessie
LABEL maintainer="Shakeel Mohamed <mmmshakeel@gmail.com>"

# update image and install necessary packages
RUN \
    apt-get update && \
    apt-get install -y --no-install-recommends curl wget apt-transport-https lsb-release ca-certificates apt-utils && \
    wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && \
    echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list \
    && echo "deb [check-valid-until=no] http://archive.debian.org/debian jessie-backports main" >> /etc/apt/sources.list \
    && curl -sS https://www.dotdeb.org/dotdeb.gpg | apt-key add - \
    && apt-get clean \
    && apt-get -o Acquire::Check-Valid-Until=false update \
    && apt-get install -y php7.2-dev autoconf automake \
    && apt-get install -y --no-install-recommends \
        ffmpeg \
        git \
        wget \
        vim \
	    zip \
        unzip \
        gettext \
        php7.2-fpm \
        php7.2-cli \
        php7.2-curl \
        php7.2-gd \
        php7.2-soap \
        php7.2-imagick \
        php7.2-imap \
        php7.2-xmlrpc \
        php7.2-mysqlnd \
        php7.2-mbstring \
        php7.2-intl \
        php7.2-xml \
        php7.2-imagick \
        php7.2-bcmath \
        php-pear \
        imagemagick \
        cron \
        logrotate \
        fontconfig \
        xfonts-75dpi \
        xfonts-base

# Install xdebug
RUN wget http://xdebug.org/files/xdebug-2.6.1.tgz && \
    tar -xvzf xdebug-2.6.1.tgz && \
    cd xdebug-2.6.1 && \
    phpize && \
    ./configure && \
    make && \
    cp modules/xdebug.so /usr/lib/php/20170718

RUN export XDEBUG_CONFIG="idekey=PHPSTORM" && \
    export PHP_IDE_CONFIG="serverName=docker-server"

RUN mkdir -p /run/php

RUN  curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
    && php /tmp/composer-setup.php --install-dir=bin --filename=composer \
    && rm /tmp/composer-setup.php

COPY php.ini /etc/php/7.2/fpm/php.ini
COPY www.conf /etc/php/7.2/fpm/pool.d/www.conf
COPY php_cli.ini /etc/php/7.2/cli/php.ini

EXPOSE 9000
CMD ["/usr/sbin/php-fpm7.2", "-F", "-R"]
