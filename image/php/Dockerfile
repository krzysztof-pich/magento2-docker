FROM php:8.3-fpm

# SETUP PHP-FPM CONFIG SETTINGS (max_children / max_requests)
RUN echo 'pm.max_children = 16' >> /usr/local/etc/php-fpm.d/zz-docker.conf && \
    echo 'pm.max_requests = 500' >> /usr/local/etc/php-fpm.d/zz-docker.conf


RUN curl -Lsf 'https://go.dev/dl/go1.19.5.linux-arm64.tar.gz' | tar -C '/usr/local' -xvzf -
ENV PATH /usr/local/go/bin:$PATH
RUN go install github.com/mailhog/mhsendmail@latest
RUN cp /root/go/bin/mhsendmail /usr/bin/mhsendmail

RUN apt-get update && apt-get install -y \
    sendmail \
    bash \
    cron \
    default-mysql-client \
    git \
    gnupg \
    gzip \
    wget \
    libbz2-dev \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libmagickwand-dev \
    libmcrypt-dev \
    libonig-dev \
    libpng-dev \
    libsodium-dev \
    libssh2-1-dev \
    libwebp-dev \
    libxslt1-dev \
    libzip-dev \
    lsof \
    mailutils \
    msmtp \
    procps \
    vim \
    zip \
    default-jdk \
    default-jre \
    net-tools \
    vim \
    && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure \
    gd --with-freetype --with-jpeg --with-webp \
  && docker-php-ext-install \
    bcmath \
    calendar \
    exif \
    gd \
    gettext \
    intl \
    mbstring \
    mysqli \
    opcache \
    pcntl \
    pdo_mysql \
    soap \
    sockets \
    sodium \
    sysvmsg \
    sysvsem \
    sysvshm \
    xsl \
    zip

RUN pecl channel-update pecl.php.net && pecl install \
    redis \
    ssh2-1.3.1 \
    xdebug \
  && pecl clear-cache \
  && rm -rf /tmp/pear \
  && docker-php-ext-enable \
      redis \
      ssh2 \
      xdebug \
  && sed -i -e 's/^zend_extension/\;zend_extension/g' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini


RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer

COPY ./php.ini /usr/local/etc/php/
COPY ./docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/

WORKDIR /var/www