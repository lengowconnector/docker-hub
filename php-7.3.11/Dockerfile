FROM php:7.3.11-apache

RUN mkdir /usr/share/man/man1

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
    unzip

RUN docker-php-ext-configure \
    gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

RUN docker-php-ext-install \
    gd \
    intl \
    mbstring \
    mysqli \
    pdo_mysql \
    soap \
    xsl \
    zip \
    bcmath

# Composer install
RUN curl -sS https://getcomposer.org/installer | \
    php -- \
      --install-dir=/usr/local/bin \
      --filename=composer \
    --version=1.9.1

# Ioncube loader instal
RUN mkdir /ioncube && \
    cd /ioncube && \
    wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz && \
    tar xvf ioncube_loaders_lin_x86-64.tar.gz && \
    cd `php -i | grep extension_dir | cut -d' ' -f 5` && \
    cp /ioncube/ioncube/ioncube_loader_lin_5.6.so . && \
    rm -rf /ioncube/

# Xdebug install
RUN curl -fsSL 'https://xdebug.org/files/xdebug-2.8.0.tgz' -o xdebug.tgz \
    && tar zxvf xdebug.tgz \
    && ( \
        cd xdebug-2.8.0 \
        && phpize \
        && ./configure --enable-xdebug \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r xdebug-2.8.0 \
    && docker-php-ext-enable xdebug

# Apache configuration
RUN a2enmod rewrite
RUN chown www-data:www-data -R /var/www/html/

# PHP configuration
COPY php.ini /usr/local/etc/php/
