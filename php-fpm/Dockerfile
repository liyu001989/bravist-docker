FROM php:7.0-fpm-alpine
MAINTAINER bravist chenghuiyong1987@gmail.com

# 换为国内镜像，以加速docker image制作速度，非中国镜内用户请注释掉下一行
RUN echo 'http://mirrors.ustc.edu.cn/alpine/edge/main' > /etc/apk/repositories
RUN echo '@community http://mirrors.ustc.edu.cn/alpine/edge/community' >> /etc/apk/repositories
RUN echo '@testing http://mirrors.ustc.edu.cn/alpine/edge/testing' >> /etc/apk/repositories

RUN apk update && apk upgrade

#时区配置
ENV TIMEZONE Asia/Shanghai

#composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === 'aa96f26c2b67226a324c27919f1eb05f21c248b987e6195cad9690d5c1ff713d53020a02ac8c217dbf90a7eacc9d141d') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv /var/www/html/composer.phar /usr/bin/composer


# pdo_mysql 扩展
RUN docker-php-ext-install pdo_mysql

#mysqli
RUN docker-php-ext-install mysqli

# bamatch
RUN docker-php-ext-install bcmath

# mcrypt
RUN apk add libmcrypt-dev
RUN docker-php-ext-install mcrypt

# zlib extensio
RUN docker-php-ext-install zlib

# zip
RUN docker-php-ext-install zip

# gd
RUN apk add libjpeg-turbo-dev
RUN apk add libpng-dev
RUN docker-php-ext-install gd

# 配置php.ini
COPY config/php.ini /usr/local/etc/php/

#工作目录
RUN mkdir /mnt/www
RUN mkdir /var/opt/gitlab

WORKDIR /mnt/www

# 端口号配置
EXPOSE 9000

# 容器命令执行
CMD ["php-fpm"]
