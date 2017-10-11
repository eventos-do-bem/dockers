FROM php:5.6-fpm

# Set timezone
ENV TZ=America/Sao_Paulo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install PHP extensions
RUN apt-get update && apt-get install -y \
      curl \
      php5-gd \
      libicu-dev \
      libpq-dev \
      libmcrypt-dev \
      libmemcachedutil2 \
      php-pear \
      libxml2-dev \
      git \
      libmagickwand-dev --no-install-recommends \
    && docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
    # Install some extra PHP Extension
    && docker-php-ext-install \
      bcmath \
      bz2 \
      iconv \
      intl \
      soap \
      mbstring \
      exif \
      ftp \
      gd \
      opcache \
      mcrypt \
      pcntl \
      pdo_mysql \
      pdo_pgsql \
      pgsql \
      zip \
      # Instal memcached redis PHP ext
      #&& pecl install memcached-2.2.0 redis \
      #&& docker-php-ext-enable memcached.so redis.so \
    && pecl install imagick && docker-php-ext-enable imagick \

    # Extensions for GD
    && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
        libxpm-dev \
        libvpx-dev \

    # Install docker
    && docker-php-ext-configure gd \
      --with-gd \
      --with-freetype-dir=/usr/include/ \
      --with-png-dir=/usr/include/ \
      --with-jpeg-dir=/usr/include/ \
    && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j${NPROC} gd \

    # Install composer
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \

    # Configure TIMEZONE
    && printf '[PHP]\ndate.timezone = "America/Sao_Paulo"\n' > /usr/local/etc/php/conf.d/tzone.ini \

  # install phpunit
  #&& curl https://phar.phpunit.de/phpunit.phar -L > phpunit.phar \
  #&& chmod +x phpunit.phar \
  #&& mv phpunit.phar /usr/local/bin/phpunit \
  #&& phpunit --version

    # install cron
    && apt-get -y install rsyslog \
    && usermod -u 1000 www-data && groupmod -g 1000 www-data \

    # APT Clear
    && apt-get clean && rm -r /var/lib/apt/lists/*

WORKDIR /var/www/html