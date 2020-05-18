FROM drupal:latest

WORKDIR /var/www/html/themes

COPY business-8.x-1.7.tar.gz business-8.x-1.7.tar.gz

RUN tar -xvf business-8.x-1.7.tar.gz && chown -R www-data.www-data * && rm business-8.x-1.7.tar.gz

HEALTHCHECK curl -f http://localhost/ || exit 0

