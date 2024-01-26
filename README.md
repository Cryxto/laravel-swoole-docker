# Laravel Project Docker Compose Starter (php fpm, nginx, redis)

# How to set

## Build 

```shell
docker-compose up -d --build
```

## Enter container bash

```shell
docker exec -it pemweb bash
```


## If there is error that refer to log , use this on container bash

```shell
chown -R www-data:www-data /var/www
```
