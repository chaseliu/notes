# How to setup a full Sentry instance


## Option 1: docker-cli

See [https://hub.docker.com/_/sentry/]


## Option 2: docker-compose (Recommended)

> If this is a new database, you must complete the following steps to generate `SENTRY_SECRET_KEY` and prepare the DB before using the compose.

```bash
# DB initialization steps

$ docker run -d --name sentry-redis redis
$ docker run -d --name sentry-postgres -e POSTGRES_PASSWORD=secret -e POSTGRES_USER=sentry -v $SENTRY_DATA_DIR:/var/lib/postgresql/data postgres
$ docker run --rm sentry config generate-secret-key
$ docker run -it --rm -e SENTRY_SECRET_KEY=$SENTRY_SECRET_KEY --link sentry-postgres:postgres --link sentry-redis:redis sentry upgrade
```

Post-initialization steps:

1. Download [docker-compose.yml](./docker-compose.yml)
2. Set environment variables required by the compose file
3. Run `docker-compose up -d`
4. Scale the worker `docker-compose scale worker=4`
