# Laravel Project Docker Compose Starter (php fpm, nginx, redis)

# How to set

## Make the laravel env first

copy .env.example into .env

```shell
cp ./src/.env.example ./src/.env
```

## Build 

```shell
docker compose up -d --build
```

or 

```shell
docker-compose up -d --build
```