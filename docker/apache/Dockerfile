FROM ubuntu:latest

# Let the container know that there is no tty
ENV DEBIAN_FRONTEN noninteractive

# Install apache, PHP, and supplimentary programs. openssh-server, curl, and lynx-cur are for debugging the container.
RUN apt-get update && apt-get -y upgrade \
    && apt-get update \
    && apt-get install -y --no-install-recommends apt-utils \
       		software-properties-common \
       		python-software-properties \
       		language-pack-en-base 
#RUN locale-gen en_US.UTF-8
#ENV LC_ALL C.UTF-8
RUN LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php

RUN apt-get update && apt-get -y upgrade && apt-get -y install \
    apache2 php7.1 php7.1-mysql php7.1-dom php7.1-mbstring libapache2-mod-php7.1 php7.1-zip curl lynx-cur

# Install php-memcached
#RUN apt-get install -y php-memcached

# Install imagemagick
RUN apt-get install -y imagemagick php-imagick

# Enable apache mods.
RUN a2enmod php7.1
RUN a2enmod rewrite

# Update the PHP.ini file, enable <? ?> tags and quieten logging.
#RUN sed -i "s/short_open_tag = Off/short_open_tag = On/" /etc/php/7.1/apache2/php.ini
#RUN sed -i "s/error_reporting = .*$/error_reporting = E_ERROR | E_WARNING | E_PARSE/" /etc/php/7.1/apache2/php.ini

# Manually set up the apache environment variables
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

# Update the default apache site with the config we created.
ADD apache-config.conf /etc/apache2/sites-enabled/000-default.conf

#####################################
# Non-Root User:
#####################################

# Add a non-root user to prevent files being created with root permissions on host machine.
ARG PUID=1000
ARG PGID=1000

ENV PUID ${PUID}
ENV PGID ${PGID}

RUN groupadd -g ${PGID} user && \
    useradd -u ${PUID} -g user -m user && \
    apt-get update -yqq

COPY ./install-composer.sh /

RUN apt-get install -y wget git \
        && sh /install-composer.sh \
        && rm /install-composer.sh

# By default start up apache in the foreground, override with /bin/bash for interative.
CMD /usr/sbin/apache2ctl -D FOREGROUND
