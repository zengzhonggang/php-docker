FROM php:7.1-fpm

ENV TZ=Asia/Chongqing

RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && \
    echo "deb http://mirrors.163.com/debian/ stretch main non-free contrib" > /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian/ stretch-updates main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian/ stretch-backports main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ stretch main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ stretch-updates main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ stretch-backports main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian-security/ stretch/updates main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian-security/ stretch/updates main non-free contrib" >> /etc/apt/sources.list

#//0 => 'amqp',
#//  1 => 'bcmath',**
#//  8 => 'gd',**
#//  14 => 'mcrypt',**
#//  15 => 'memcache',
#//  16 => 'memcached',
#//  17 => 'mysqli',**
#//  20 => 'pcntl',**
#//  23 => 'pdo_mysql',**
#//  27 => 'redis',
#//  31 => 'sockets',**
#//  39 => 'Zend OPcache',** opcache
#//  40 => 'zip', **

RUN apt-get update && apt-get install -y \
		libjpeg62-turbo-dev \
		libmcrypt-dev \
		libpng-dev \
	&& docker-php-ext-install -j$(nproc) mcrypt gd bcmath mysqli pdo_mysql sockets opcache zip

RUN apt-get update && apt-get install -y \
		libfreetype6-dev \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd

RUN apt-get update && apt-get install -y libmemcached-dev zlib1g-dev \
    && pecl install memcached-3.0.4 \
    && docker-php-ext-enable memcached

RUN apt-get update \
    && pecl install redis-4.1.0 \
    && pecl install xdebug-2.6.0 \
    && docker-php-ext-enable redis xdebug

RUN apt-get update && apt-get install -y librabbitmq-dev \
    && pecl install amqp-1.9.3 \
    && docker-php-ext-enable amqp

RUN apt-get update && apt-get install -y \
    iputils-ping \
    libnet-ifconfig-wrapper-perl
#    ntpdate \
#    libconfig-crontab-perl

COPY ./config/php-fpm.conf /usr/local/etc/php-fpm.conf

COPY ./config/www.conf /usr/local/etc/php-fpm.d/www.conf

COPY ./config/php.ini /usr/local/etc/php/

COPY ./config/log.conf /usr/local/etc/php-fpm.d/zz-log.conf

RUN apt-get update && apt-get install -y libhiredis-dev

RUN curl -fsSL 'https://github.com/nrk/phpiredis/archive/v1.0.0.tar.gz' -o phpiredis.tar.gz \
    && mkdir -p phpiredis \
    && tar -xf phpiredis.tar.gz -C phpiredis --strip-components=1 \
    && rm phpiredis.tar.gz \
    && ( \
        cd phpiredis \
        && phpize \
        && ./configure --enable-phpiredis \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r phpiredis \
    && docker-php-ext-enable phpiredis
