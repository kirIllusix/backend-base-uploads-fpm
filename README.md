# backend-base-uploads-fpm
```
FROM php:7.0.33-fpm

# disable interactive functions
ENV DEBIAN_FRONTEND noninteractive

# Copy timezone configration
COPY ./src/timezone.ini /usr/local/etc/php/conf.d/timezone.ini

# Set timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/Europe/Kiev /etc/localtime
RUN "date"

# Copy opcache configration
COPY ./src/opcache.ini /usr/local/etc/php/conf.d/opcache.ini

# Composer
ENV COMPOSER_HOME /var/www/.composer

RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/bin \
    --filename=composer

RUN apt autoremove -y \
	&& apt clean -y \
	&& rm -rf /tmp/* /var/tmp/* \
	&& find /var/cache/apt/archives /var/lib/apt/lists -not -name lock -type f -delete \
	&& find /var/cache -type f -delete

RUN mkdir -p /var/www/uploads-html && chown www-data:www-data /var/www/uploads-html && chmod 777 /var/www/uploads-html

VOLUME /var/www/uploads-html
WORKDIR /var/www/uploads-html

```
