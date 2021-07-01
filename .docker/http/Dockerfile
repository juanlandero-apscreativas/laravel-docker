FROM php:7.3-apache-stretch

ARG DEBIAN_FRONTEND=noninteractive

# system dependencies
RUN apt-get update \
    && apt-get install -qq --yes --no-install-recommends \
    apt-utils \
    ca-certificates \
    libxml2-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libxpm-dev \
    libwebp-dev \
    libzip-dev \
    libcurl4-openssl-dev \
    file \
    g++ \
    make \
    uuid-runtime \
    git \
    zip \
    unzip \
    xvfb \
    wkhtmltopdf \
    && apt-get autoremove \
    && apt-get clean \
    && apt-get autoclean

# configure gd extension
RUN docker-php-ext-configure gd \
    --with-gd \
    --with-webp-dir \
    --with-xpm-dir \
    --with-png-dir \
    --with-zlib-dir \
    --with-freetype-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/

# php extensions
RUN docker-php-ext-install -j$(nproc) \
    gd \
    pdo_mysql \
    opcache \
    zip
    
# Imagick Extensio
RUN apt-get update && apt-get install -y libmagickwand-dev --no-install-recommends && rm -rf /var/lib/apt/lists/*
RUN printf "\n" | pecl install imagick
RUN docker-php-ext-enable imagick

# enable apache modules and
# test apache configuration
RUN a2enmod rewrite &&\
    apache2ctl configtest

# expose http and https ports
EXPOSE 80

# command
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
