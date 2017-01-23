FROM php:7.0-fpm

RUN apt-get update --fix-missing && \
#INSTALL TOOLS
    apt-get install -y \
        apt-utils \
        git \
        wget \
        curl \
        libpq-dev \
        libicu-dev \
        libcurl4-gnutls-dev \
        libmcrypt-dev \
        libmemcached-dev \
        npm \
        libxml2-dev \
        libpng-dev \
    && \
    docker-php-ext-install -j$(nproc) \
        mysqli \
        pdo \
        pdo_mysql \
        opcache \
        zip \
        mbstring \
        bcmath \
        pcntl \
        mcrypt \
        soap \
        gd

#Possible values for ext-name:
#bcmath bz2 calendar ctype curl dba dom enchant exif fileinfo filter ftp gd gettext gmp hash iconv imap interbase intl
#json ldap mbstring mcrypt mysqli oci8 odbc opcache pcntl pdo pdo_dblib pdo_firebird pdo_mysql pdo_oci pdo_odbc pdo_pgsql
#pdo_sqlite pgsql phar posix pspell readline recode reflection session shmop simplexml snmp soap sockets spl standard
#sysvmsg sysvsem sysvshm tidy tokenizer wddx xml xmlreader xmlrpc xmlwriter xsl zip


RUN  \
#COMPOSER
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/bin --filename=composer && \
# ConfD
    wget https://github.com/kelseyhightower/confd/releases/download/v0.10.0/confd-0.10.0-linux-amd64 -O /usr/local/bin/confd && \
    chmod +x /usr/local/bin/confd && \

# Swagger Checker
    ln -s /usr/bin/nodejs /usr/bin/node && \
    npm install -g swagger-cli && \

# CODE COVERAGE
    git clone https://github.com/michaelmoussa/php-coverage-checker.git /tmp/php-checker && \
    mv /tmp/php-checker/coverage-checker.php /usr/local/bin/coverage-checker && \
    chmod +x /usr/local/bin/coverage-checker && \
    rm -rf /tmp/php-checker && \

# XDEBUG
    mkdir /tmp/xdebug_install && \
    wget -O /tmp/xdebug_install/xdebug-2.4.0.tgz http://xdebug.org/files/xdebug-2.4.0.tgz && \
    cd /tmp/xdebug_install/ && \
    tar -xvzf xdebug-2.4.0.tgz && \
    cd xdebug-2.4.0 && \
    phpize && \
    ./configure && \
    make && \
    cp modules/xdebug.so /usr/local/lib/php/extensions/no-debug-non-zts-20151012/ && \
    cd / && \
    rm -rf /tmp/xdebug_install

# COMPOSER
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ADD bin/install.sh /usr/local/bin/install
RUN chmod +x /usr/local/bin/install

ADD ext-xdebug.ini  /usr/local/etc/php/conf.d/ext-xdebug.ini
ADD php-custom.ini /usr/local/etc/php/conf.d/php-custom.ini

#################### <<<ENV ####################

ENV PHP_IDE_CONFIG serverName=nginad.local

#################### >>>ENV ####################

#################### Alias #####################
RUN echo "alias l='ls $LS_OPTIONS -lA'" >> /root/.bashrc && \
    echo "alias lsa='ls -alh'" >> /root/.bashrc && \
    echo "alias ll='ls -alh'" >> /root/.bashrc && \
    echo 'export TERM=xterm' >> ~/.bashrc && \
    echo "export VISIBLE=now" >> /etc/profile

################# >>>SSH ###############
RUN apt-get install -y --force-yes openssh-server

# SSH
RUN mkdir /var/run/sshd && \
    echo 'root:root' | chpasswd && \
    sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd && \
    echo "export VISIBLE=now" >> /etc/profile

################# >>>SET UP ###############
#RUN cd /var/www/nginad.local/public

#RUN bower install --allow-root

##################LOG and CACHE for laravel ######
#RUN chown -R www-data:www-data /var/www/hitcash.local/storage
