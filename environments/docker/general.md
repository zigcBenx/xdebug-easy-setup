# Disclaimer
depending to your docker nevironment setup folowing instructions my vary, but for most cases those steps should work.

# Install
Install xdebug in the container that is running your app. You can install it in your Dockerfile.

add following lines, which will install and enable xdebug php extension.

```bash
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
```


You should already have php.ini somewhere in your docker dev env setup. In same location create another file and name it `xdebug.ini` inside you will have all xdebug configuration. Paste content bellow, and adjust later by your needs.

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

# COPY xdebug configuration to container
```bash
COPY ./xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
```


Add this to docker-compose.yaml file to the service that is rnning your app php-fpm in my example.

()THIS IS FOR CLI AND TESTS ONLY??()the server name is so the IDE and xdebug know throw what keyword they are suppose to communicate
```yaml
    expose:
      - "9000"
    environment:
      PHP_IDE_CONFIG: "serverName=invoiceshelf.test"
```


**Optional but recommended:**
You can also add this line to same service under `volumnes`. This will make live changes in container as you change the xdebug.ini file on you machine.
So you don't have to rebuild container everytime you make a change.

```yaml
    volumes:
      - ../:/home/invoiceshelf/app
      - ./php/xdebug.ini:/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
```