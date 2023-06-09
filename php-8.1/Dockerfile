FROM php:8.1.20-cli-alpine

LABEL maintainer="info@freely.agency" \
    org.label-schema.schema-version="1.0" \
    org.label-schema.build-date=$BUILD_DATE \
    org.label-schema.vcs-ref=$VCS_REF \
    org.label-schema.name="freelyagency/drupal-phpcs" \
    org.label-schema.description="Drupal PHP Code Sniffer Standards inside Docker" \
    org.label-schema.url="https://github.com/FreelyAgency/docker-drupal-phpcs" \
    org.label-schema.vcs-url="https://github.com/FreelyAgency/docker-drupal-phpcs" \
    org.label-schema.vendor="Freely Agency" \
    org.label-schema.docker.cmd="docker run --rm -it -v $(pwd):/data freelyagency/drupal-phpcs"

# Install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php  && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer

# Allow Composer to be run as root and set $PATH for Composer Executables
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV PATH "$PATH:/root/.composer/vendor/bin"

# Install CodeSniffer
RUN composer global config --no-plugins allow-plugins.dealerdirect/phpcodesniffer-composer-installer true && \
    composer global require "squizlabs/php_codesniffer"

# Install Drupal Coding Standards and set as default_standard
RUN composer global require drupal/coder:8.3.13 && \
    phpcs --config-set installed_paths /root/.composer/vendor/drupal/coder/coder_sniffer/ \
    && phpcs --config-set default_standard Drupal,DrupalPractice

WORKDIR /data

CMD ["phpcs"]