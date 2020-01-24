FROM ubuntu:18.04

ENV TZ=America/Los_Angeles
ENV APACHE_RUN_USER=www-data
ENV APACHE_RUN_GROUP=www-data
ENV APACHE_LOG_DIR=/var/log/apache2
ENV APACHE_LOCK_DIR=/var/lock/apache2
ENV APACHE_PID_FILE=/var/run/apache2.pid

RUN set -x \
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
    && echo $TZ > /etc/timezone \
    && apt-get update \
    && apt-get install -y curl php sudo apache2 php libapache2-mod-php git php-curl php-xml php-mbstring php-zip npm \
    && apt-get clean \
    && sudo adduser admin --gecos "First Last,RoomNumber,WorkPhone,HomePhone" --disabled-password \
    && su admin \
    && sudo a2enmod rewrite \
    && cd /var/www \
    && git clone https://github.com/njm222/Offerista-Demo.git \
    && cd ./Offerista-Demo \
    && curl https://get.symfony.com/cli/installer -O - | bash \
    && curl -sS https://getcomposer.org/installer -o composer-setup.php \
    && sudo php composer-setup.php \
    && php composer.phar install \
    && sudo npm install -g yarn \
    && yarn install \
    && yarn encore production
    

EXPOSE 80

ADD apache-config.conf /etc/apache2/sites-enabled/000-default.conf

CMD ["apachectl", "-D", "FOREGROUND"]