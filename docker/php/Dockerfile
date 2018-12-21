FROM php:7.2.5-fpm

WORKDIR /var/www

RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    bison \
    libpq-dev \
    libxml2-dev \
    libicu-dev \
    libpng-dev \
    libcurl4-gnutls-dev \
    libbz2-dev \
    libssl-dev \
    libmagickwand-dev \
    apt-transport-https \
    build-essential \
    ca-certificates \
    curl \
    python \
    rsync \
    cron \
    supervisor \
    wget \
    locales \
    software-properties-common \
    zlib1g-dev \
    haproxy \
    telnet \
    libpng-dev libcurl4-gnutls-dev \
    openssl libssl-dev \
    && docker-php-ext-install -j$(nproc) opcache iconv mysqli pdo_mysql bcmath \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) bcmath \
    && sed -i 's/pm.max_children = [0-9]\{1,\}/pm.max_children = 2000/g' /usr/local/etc/php-fpm.d/www.conf \
    && curl -fsSL http://pecl.php.net/get/redis-4.0.2.tgz -o redis.tgz \
    && mkdir -p redis \
    && tar -xf redis.tgz -C redis --strip-components=1 \
    && rm redis.tgz \
    && ( \
    cd redis \
    && phpize \
    && ./configure --with-php-config=/usr/local/bin/php-config \
    && make -j$(nproc) \
    && make install \
    ) \
    && docker-php-ext-enable redis \
    && rm -r redis

RUN docker-php-ext-install pgsql \
    pdo_pgsql \
    opcache \
    calendar \
    zip \
    curl \
    soap \
    bz2 \
    intl \
    sockets

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

RUN docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache

COPY ./php.ini /usr/local/etc/php/

WORKDIR /var/www