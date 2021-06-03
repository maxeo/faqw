FROM debian:buster

RUN \
  apt-get update && \
  apt-get install -y \
  curl \
  wget \
  git \
  apt-utils \
  vim \
  nano \
  locales \
  iptables \
  libapache2-mod-php7.3 \
  php7.3-mysql \
  php7.3-gd \
  php7.3-imagick \
  php7.3-dev \
  php7.3-curl \
  php7.3-opcache \
  php7.3-cli \
  php7.3-sqlite \
  php7.3-intl \
  php7.3-tidy \
  php7.3-imap \
  php7.3-json \
  php7.3-pspell \
  php7.3-recode \
  php7.3-common \
  php7.3-sybase \
  php7.3-sqlite3 \
  php7.3-bz2 \
  php7.3-common \
  php7.3-memcached \
  php7.3-memcache \
  php7.3-redis \
  php7.3-xml \
  php7.3-shmop \
  php7.3-mbstring \
  php7.3-zip \
  php7.3-bcmath \
  php7.3-soap \
  php7.3-ldap \
  php7.3-xdebug \
  sudo \
  unzip

#


RUN echo "xdebug.remote_enable = 1\nxdebug.remote_autostart = 1\nxdebug.remote_port = 9005" >> /etc/php/7.3/mods-available/xdebug.ini

# installazione ssmtp - non diposnibile su buster al momento
RUN apt-get install -y libgnutls-openssl27
RUN wget http://ftp.us.debian.org/debian/pool/main/s/ssmtp/ssmtp_2.64-8+b2_amd64.deb
RUN dpkg -i ssmtp_2.64-8+b2_amd64.deb



# installazione Symfony
RUN  wget https://get.symfony.com/cli/installer -O - | bash && \
    mv /root/.symfony/bin/symfony /usr/local/bin/symfony

# installazione Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV TZ=Europe/Rome
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#HEALTHCHECK --interval=30s --timeout=3s CMD curl --fail http://localhost:80 || exit 1
#HEALTHCHECK --interval=30s --timeout=3s CMD wget --quiet --tries=1 --spider http://localhost/ || exit 1


RUN a2enmod rewrite && \
    a2enmod remoteip && \
    a2enmod ssl && \
    service apache2 restart

ENV TZ=Europe/Rome

HEALTHCHECK --interval=30s --timeout=3s CMD curl --fail http://localhost:80 || exit 1



ENTRYPOINT ["/bin/bash", "-c"]

CMD ["/run.sh"]