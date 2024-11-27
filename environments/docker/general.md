# Disclaimer
Depending on your Docker environment setup, the following instructions may vary, but these steps should work for most cases.

# Installation
Install Xdebug in the container that runs your application. You can add this to your Dockerfile by including the following lines to install and enable the Xdebug PHP extension:

```bash
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
```

# Configuration
You should already have a `php.ini` file somewhere in your Docker development environment setup. In the same location, create a new file named `xdebug.ini` that will contain all Xdebug configurations. Add the following content and adjust it according to your needs:

```ini
[xdebug]
zend_extension=xdebug
xdebug.mode=debug
xdebug.start_with_request=yes
xdebug.client_host=172.17.0.1
xdebug.client_port=9000
xdebug.idekey=PHPSTORM
xdebug.log=/var/log/xdebug.log
```

# Docker Setup

## Copy Xdebug Configuration
Add this line to your Dockerfile to copy the Xdebug configuration into the container:

```bash
COPY ./xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
```

## Docker Compose Configuration
Add the following to your `docker-compose.yaml` file in the service that runs your PHP-FPM application:

```yaml
expose:
  - "9000"
environment:
  PHP_IDE_CONFIG: "serverName=invoiceshelf.test"
```

Note: The `serverName` configuration is required for CLI and tests only, as it helps the IDE and Xdebug communicate using the specified keyword.

## Optional but Recommended
Add this line under the `volumes` section of the same service for live configuration updates. This eliminates the need to rebuild the container when you modify the `xdebug.ini` file:

```yaml
volumes:
  - ../:/home/invoiceshelf/app
  - ./php/xdebug.ini:/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
```
