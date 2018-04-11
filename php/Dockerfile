FROM php:7.2-fpm

RUN apt-get update && apt-get install -y \
curl \
wget \
nano \
git \
libfreetype6-dev \
libjpeg62-turbo-dev \
libxslt-dev \
libicu-dev \
libmcrypt-dev \
libxml2-dev \

&& docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip \
&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl
RUN docker-php-ext-install xsl
RUN docker-php-ext-install soap
ADD exteansions.sh /
RUN /bin/sh /exteansions.sh

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini

WORKDIR /var/www

CMD ["php-fpm"]
