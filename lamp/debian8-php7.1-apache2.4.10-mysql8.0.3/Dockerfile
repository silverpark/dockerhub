FROM mysql:8.0.3
MAINTAINER Daniel Boetsch

#################
# Install utils #
#################

RUN \
    apt-get update \
    && apt-get -y install wget \
                curl \
                apt-utils \
                telnet \
                vim \
                supervisor \
                unzip \
                postgresql-client-9.4 \
                locales \
    && dpkg-reconfigure -f noninteractive locales \
    && locale-gen C.UTF-8 \
    && /usr/sbin/update-locale LANG=C.UTF-8 \
    && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
    && echo "en_US UTF-8" >> /etc/locale.gen \
    && echo "en_GB.UTF-8 UTF-8" >> /etc/locale.gen \
    && echo "en_GB UTF-8" >> /etc/locale.gen \
    && echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen \
    && echo "fr_FR UTF-8" >> /etc/locale.gen \
    && locale-gen \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

##################
# Install php7.1 #
##################

RUN \
    apt-get update \
    && apt-get install -y apt-transport-https lsb-release ca-certificates \
    && wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
    && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list \
    && apt-get update \
    && apt-get -y install -y php7.1-cli \
        php7.1-fpm \
        php7.1-pgsql \
        php7.1-curl \
        php7.1-mbstring \
        php7.1-zip \
        php7.1-xml \
        php7.1-ssh2

RUN mkdir /run/php

###############
## Apache
###############

RUN \
    apt-get update \
    && apt-get install -y apache2 apache2-mpm-worker libapache2-mod-php7.1 php7.1-mysql \
    && a2enmod actions proxy proxy_fcgi ssl rewrite headers expires \
    && rm -f /etc/apache2/sites-enabled/*

# /!\ because it does not work with volume /!\
COPY apache2/000-default.conf /etc/apache2/sites-enabled/000-default.conf

#ENV APACHE_RUN_USER www-data
#ENV APACHE_RUN_GROUP www-data
#ENV APACHE_LOG_DIR /var/log/apache2

EXPOSE 80 443

#######
# LOG #
#######

RUN ln -sf /dev/stderr /var/log/php7.1-fpm.log
#ln -sf /dev/stdout /var/log/nginx/morpheus-simulate.access.log && \
#ln -sf /dev/stderr /var/log/nginx/morpheus-simulate.error.log

############
# TIMEZONE #
############

ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

###############
# Supervisord #
###############

COPY supervisord/supervisord.conf /supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-c", "/supervisord.conf"]
