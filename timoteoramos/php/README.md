# PHP

## Features

- Composer installed by default
- Apache 2 with mod_rewrite enabled

## Examples

### Legacy PHP website

`Dockerfile` example:

```dockerfile
FROM docker.io/timoteoramos/php:8.1 AS production

# Install the modules that you need via Alpine repository (they've a lot of plugins available)
RUN apk add php81-gd php81-mysqli php81-pecl-redis

# This is my recommended non-root user, since the working directory is the apache public folder
USER apache

# Copy your public folder to htdocs (remember to change the ownership to apache:apache)
COPY --chown apache:apache ./src /var/www/localhost/htdocs
```

### Laravel project

Recommended `.dockerignore`:

```text
src/node_modules
src/vendor
src/package*.json
src/vite.config.js
src/yarn.lock
```

`Dockerfile` example:

```dockerfile
FROM docker.io/timoteoramos/php:8.1 AS production

# Change the default Apache directory and install the required Laravel modules
RUN sed 's/htdocs/public/g' /etc/apache2/httpd.conf && \
    apk add php81-dom php81-fileinfo php81-session php81-tokenizer php81-xml php81-xmlwriter

# Optional: install the recommended Laravel packages
RUN apk add php81-gd php81-ftp php81-intl php81-pcntl php81-pecl-apcu php81-pecl-memcached php81-pecl-redis php81-pdo_sqlite php81-posix

# Optional: install the recommended monolog packages
RUN apk add php81-pecl-amqp php81-pecl-mongodb php81-sockets

# Optional: install the recommended ramsey/uuid packages
RUN apk add php81-bcmath php81-gmp

# This is my recommended non-root user, since the working directory is the apache public folder
USER apache

# Send your composer files first (in order to improve your local builds)
COPY --chown apache:apache ./src/composer.* /var/www/localhost/

# Run composer
RUN composer -d /var/www/localhost update && composer -d /var/www/localhost install --no-dev

# Copy your public folder to htdocs (remember to change the ownership to apache:apache)
COPY --chown apache:apache ./src /var/www/localhost
```
