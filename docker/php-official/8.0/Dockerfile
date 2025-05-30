#+++++++++++++++++++++++++++++++++++++++
# Dockerfile for webdevops/php-official:8.0
#    -- automatically generated  --
#+++++++++++++++++++++++++++++++++++++++


FROM php:8.0-fpm-bookworm

LABEL maintainer=info@webdevops.io \
      vendor=WebDevOps.io \
      io.webdevops.layout=8 \
      io.webdevops.version=1.5.0

ENV TERM="xterm" \
    LANG="C.UTF-8" \
    LC_ALL="C.UTF-8"
ENV DOCKER_CONF_HOME=/opt/docker/ \
    LOG_STDOUT="" \
    LOG_STDERR=""
ENV APPLICATION_USER=application \
    APPLICATION_GROUP=application \
    APPLICATION_PATH=/app \
    APPLICATION_UID=1000 \
    APPLICATION_GID=1000
ENV PHP_SENDMAIL_PATH="/usr/sbin/sendmail -t -i"
ENV COMPOSER_VERSION="2"


# Baselayout copy (from staged image)
COPY --from=webdevops/toolbox /baselayout/sbin/* /sbin/
COPY --from=webdevops/toolbox /baselayout/usr/local/bin/* /usr/local/bin/


COPY conf/ /opt/docker/

RUN set -x \
    # Init bootstrap
    && apt-update \
    && /usr/local/bin/generate-dockerimage-info \
    # Enable non-free
    && sed -ri "s/(deb.*\/debian $(docker-image-info dist-codename) main)/\1 contrib non-free /" -- /etc/apt/sources.list.d/debian.sources \    
    && apt-update \
    # System update
    && /usr/local/bin/apt-upgrade \
    # Base stuff
    && apt-install \
        apt-transport-https \
        ca-certificates \
        locales \
        gnupg

RUN set -x \
    # Install packages
    && chmod +x /opt/docker/bin/* \
    && apt-install \
        supervisor \
        wget \
        curl \
        vim \
        net-tools \
        tzdata \
    && chmod +s /sbin/gosu \
    && docker-run-bootstrap \
    && docker-image-cleanup

RUN set -x \
    # Install services
    && apt-install \
        # Install common tools
        zip \
        unzip \
        bzip2 \
        moreutils \
        dnsutils \
        openssh-client \
        rsync \
        git \
        patch \
    && /usr/local/bin/generate-locales \
    && docker-run-bootstrap \
    && docker-image-cleanup

RUN set -x \
    # Install php environment
    && apt-install \
        # Install tools
        imagemagick \
        graphicsmagick \
        ghostscript \
        jpegoptim \
        libjpeg-turbo-progs \
        pngcrush \
        optipng \
        apngopt \
        pngnq \
        pngquant \
        # Libraries
        libvips42 \
        librabbitmq4 \
        libldap-2.5-0 \
        libxslt1.1 \
        libldap-common \
        zlib1g \
        libpq5 \
        libpng16-16 \
        libmcrypt4 \
        libzip4 \
        # Dev and headers
        libjpeg62-turbo-dev \
        libwebp-dev \
        libfreetype6-dev \
        libbz2-dev \
        libicu-dev \
        libldap2-dev \
        libldb-dev \
        libmcrypt-dev \
        libxml2-dev \
        libxslt1-dev \
        zlib1g-dev \
        libmemcached-dev \
        libpng-dev \
        libpq-dev \
        libzip-dev \
        libc-client-dev \
        libc-client2007e-dev \
        libkrb5-dev \
        libvips-dev \
        librabbitmq-dev \
        libmagick++-dev \
        libgmp-dev \
    # Install guetzli
    && wget https://github.com/google/guetzli/archive/master.zip \
    && unzip master.zip \
    && make -C guetzli-master \
    && cp guetzli-master/bin/Release/guetzli /usr/local/bin/ \
    && rm -rf master.zip guetzli-master \
    # Install new version of ICU
    && curl -sS -o /tmp/icu.tar.gz -L https://github.com/unicode-org/icu/releases/download/release-73-1/icu4c-73_1-src.tgz \
    && tar -zxf /tmp/icu.tar.gz -C /tmp && cd /tmp/icu/source && ./configure --prefix=/usr/local && make && make install && cd / && rm -rf /tmp/icu* \
    # Install extensions
    && PKG_CONFIG_PATH=/usr/local docker-php-ext-configure intl \
    && docker-php-ext-configure gd --with-jpeg --with-freetype --with-webp \
    && git clone --branch latest --depth 1 https://github.com/php-amqp/php-amqp.git /usr/src/php/ext/amqp \
    && cd /usr/src/php/ext/amqp && git submodule update --init \
    && git clone --branch master --depth 1 https://github.com/Imagick/imagick.git /usr/src/php/ext/imagick \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && PHP_OPENSSL=yes docker-php-ext-configure imap --with-kerberos --with-imap-ssl \    
    && docker-php-ext-install \
        bcmath \
        amqp \
        imagick \
        ffi \
        bz2 \
        calendar \
        exif \
        ftp \
        gmp \
        intl \
        gettext \
        ldap \
        mysqli \
        imap \
        pcntl \
        pdo_mysql \
        pdo_pgsql \
        pgsql \
        soap \
        sockets \
        tokenizer \
        sysvmsg \
        sysvsem \
        sysvshm \
        shmop \
        xsl \
        zip \
        gd \
        gettext \
        opcache \
    && pecl install memcached \
    && pecl install apcu \
    && pecl install excimer \
    && pecl install opentelemetry \
    && pecl install protobuf \
    && pecl install xmlrpc-1.0.0RC3 \
    && pecl install vips \
    && pecl install redis \
    && pecl install mongodb \
    && echo extension=apcu.so > /usr/local/etc/php/conf.d/apcu.ini \
    && echo extension=excimer.so > /usr/local/etc/php/conf.d/excimer.ini \
    && echo extension=opentelemetry.so > /usr/local/etc/php/conf.d/opentelemetry.ini \
    && echo extension=protobuf.so > /usr/local/etc/php/conf.d/protobuf.ini \
    && echo extension=redis.so > /usr/local/etc/php/conf.d/redis.ini \
    && echo extension=mongodb.so > /usr/local/etc/php/conf.d/mongodb.ini \
    && echo extension=memcached.so > /usr/local/etc/php/conf.d/memcached.ini \
    && echo extension=vips.so > /usr/local/etc/php/conf.d/vips.ini \
    && echo extension=xmlrpc.so > /usr/local/etc/php/conf.d/xmlrpc.ini \
    # Uninstall dev and header packages
    && apt-get purge -y -f --force-yes \
        libc-client-dev \
        libkrb5-dev \
        libbz2-dev \
        libicu-dev \
        libldap2-dev \
        libldb-dev \
        libmcrypt-dev \
        libxml2-dev \
        libxslt1-dev \
        zlib1g-dev \
        libpng-dev \
        libwebp-dev \
        libpq-dev \
        libzip-dev \
        libvips-dev \
        librabbitmq-dev \
        libmagick++-dev \
        libgmp-dev \
    && rm -f /usr/local/etc/php-fpm.d/zz-docker.conf \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer2 \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer1 --1 \
    && ln -sf /usr/local/bin/composer2 /usr/local/bin/composer \
    # Enable php services
    && docker-service enable syslog \
    && docker-service enable cron \
    && docker-run-bootstrap \
    && docker-image-cleanup

WORKDIR /
EXPOSE 9000
ENTRYPOINT ["/entrypoint"]
CMD ["supervisord"]
