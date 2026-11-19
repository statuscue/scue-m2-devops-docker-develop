FROM php:8.1-fpm
LABEL authors="statuscue"

ARG GITHUB_TOKEN
ARG AMASTY_USERNAME
ARG AMASTY_PASSWORD
ARG MAGENTO_USERNAME
ARG MAGENTO_PASSWORD
ARG MAGENTO_USER_WEB
ARG MAGENTO_UID_WEB

RUN apt-get update && apt-get install -y \
    git unzip libicu-dev libxml2-dev libxslt-dev libzip-dev libonig-dev \
    libpng-dev libjpeg-dev libfreetype6-dev zlib1g-dev libssl-dev \
    libssh-dev openssh-client mariadb-client mc \
    && docker-php-ext-install \
        intl xsl zip pdo_mysql soap sockets bcmath opcache \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd

RUN apt-get install -y mc

RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

RUN composer config --global github-oauth.github.com ${GITHUB_TOKEN}
RUN composer config --global http-basic.composer.amasty.com ${AMASTY_USERNAME} ${AMASTY_PASSWORD}
RUN composer config --global http-basic.repo.magento.com ${MAGENTO_USERNAME} ${MAGENTO_PASSWORD}

RUN mkdir -p /root/.ssh && \
    ssh-keyscan github.com >> /root/.ssh/known_hosts

RUN apt-get install -y sudo
RUN useradd -m -u ${MAGENTO_UID_WEB} -s /bin/bash ${MAGENTO_USER_WEB} && \
    echo "${MAGENTO_USER_WEB} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
RUN chown -R ${MAGENTO_USER_WEB}:www-data /var/www/html

USER ${MAGENTO_USER_WEB}
WORKDIR /var/www/html

