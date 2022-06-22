FROM php:8.1-apache

RUN mkdir /usr/share/man/man1; exit 0

RUN apt-get update \
    && apt-get install -y \
    ant \
    cron \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libxslt1-dev \
    default-mysql-client \
    wget \
    zip \
    libzip-dev \
    libonig-dev \
    unzip

RUN docker-php-ext-configure gd --with-freetype --with-jpeg

RUN docker-php-ext-install \
    gd \
    intl \
    mysqli \
    pdo_mysql \
    soap \
    xsl \
    zip \
    bcmath \
    sockets

# Composer install
RUN curl -sS https://getcomposer.org/installer | \
    php -- \
      --install-dir=/usr/local/bin \
      --filename=composer \
    --version=2.1.0

# Ioncube loader instal
RUN mkdir /ioncube && \
    cd /ioncube && \
    wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz && \
    tar xvf ioncube_loaders_lin_x86-64.tar.gz && \
    cd `php -i | grep extension_dir | cut -d' ' -f 5` && \
    cp /ioncube/ioncube/ioncube_loader_lin_5.6.so . && \
    rm -rf /ioncube/

# Xdebug install
RUN curl -fsSL 'https://xdebug.org/files/xdebug-3.1.5.tgz' -o xdebug.tgz \
    && tar zxvf xdebug.tgz \
    && ( \
        cd xdebug-3.1.5 \
        && phpize \
        && ./configure --enable-xdebug \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r xdebug-3.1.5 \
    && docker-php-ext-enable xdebug

# Apache configuration
RUN a2enmod rewrite
RUN chown www-data:www-data -R /var/www/html/

# PHP configuration
COPY php.ini /usr/local/etc/php/
