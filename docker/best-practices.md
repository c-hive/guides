# Docker / Best practices

## Goals

1, `docker-compose up` should create a working or failed state. E.g. if adding new dependencies or migrations, often `docker-compose up` would work but the app would throw errors about missing dependencies or migrations.

2, Minimal setup time. E.g. cache dependencies.

#### Use separate files for separate environments and services when needed

GOOD

```yml
# .docker/docker-compose.dev.yml
version: '3'
services:

  web:
    build:
      context: ../
      dockerfile: .docker/web.dev.Dockerfile
  
  cache:
    build:
      context: ../
      dockerfile: .docker/web.prod.Dockerfile
```

#### Use entrypoints to provide ready-to-use services

GOOD

`entrypoint.sh`
```sh
#!/bin/sh
set -e

wait_for()
{
  echo "Waiting $1 seconds for $2:$3"
  timeout $1 sh -c 'until nc -z $0 $1; do sleep 0.1; done' $2 $3
  echo "$2:$3 available"
}

wait_for 10 db 5432
wait_for 10 redis 6379

bin/rails db:prepare

exec "$@"
```

BAD

```sh
docker-compose run web bin/rails db:prepare
```

#### Use entrypoints to install dependencies during runtime and cache them via volumes

For local development. A better alternative would be build-time caching, but those do not transfer to runtime, not even with buildkit.

GOOD

`entrypoint.sh`
```sh
#!/bin/sh
set -e

npm install

exec "$@"
```

`docker-compose.yml`
```yml
services:
  node-app:
    volumes:
      - node-app_npm_cache:/app/node_modules

volumes:
    # Named volumes are used so that `dc run ...` doesn't re-initialize the volumes
    node-app_npm_cache:
```

BAD

`Dockerfile`
```Dockerfile
RUN npm install
```

#### Combine non-hanging entrypoints with docker-compose commands

Use entrypoints to ensure that the container is ready to use and use `command` to start the process. This way the entrypoint doesn't hang and it's possible to also use `run`.

GOOD

`Dockerfile`
```Dockerfile
COPY . ./
RUN chmod +x entrypoint.sh
ENTRYPOINT ["./entrypoint.sh"]
```

`entrypoint.sh`
```sh
exec "$@"
```

`docker-compose.yml`
```sh
services:
  web:
    command: 'bin/rails server'
```

Note that
- if using `ENTRYPOINT entrypoint.sh`, `exec "$@"` won't work, instead use `ENTRYPOINT ["./entrypoint.sh"]`
- using `ENTRYPOINT ["entrypoint.sh"]` won't work, instead use `ENTRYPOINT ["./entrypoint.sh"]`

BAD

`entrypoint.sh`
```sh
bin/rails server
```

#### Do not put 1-time setup in entrypoint

It would run every time, even if running `docker run`.

GOOD

`.docker/web-entrypoint.sh`
```sh
wait_for-it db:5432
rake db:prepare # Only seeds DB if it had to be created
exec "$@"
```

BAD

`.docker/web-entrypoint.sh`
```sh
wait_for-it db:5432
rake db:seed
exec "$@"
```

#### Use .dockerignore and whitelist needed files

```sh
# Ignore everything
**

# Allow files and directories
!/app
!/COPYRIGHT.txt
```

See also:
- https://codefresh.io/docker-tutorial/not-ignore-dockerignore/
- https://youknowfordevs.com/2018/12/07/getting-control-of-your-dockerignore-files.html
- https://stackoverflow.com/questions/28097064/dockerignore-ignore-everything-except-a-file-and-the-dockerfile

#### Keep setup in `.docker` folder

*Note: [Heroku doesn't support this](https://stackoverflow.com/questions/57745231/using-a-dockerfile-from-a-subfolder-in-heroku)*

```yml
# .docker/docker-compose.yml
version: '3'
services:

  web:
    build:
      context: ../
      dockerfile: .docker/Dockerfile
```

#### Avoid `container_name` option

> Because Docker container names must be unique, you cannot scale a service beyond 1 container if you have specified a custom name. Attempting to do so results in an error.

https://docs.docker.com/compose/compose-file/#container_name

> If we were to do it all over again, this (`container_name`) would definitely not be in the spec. It's constantly misused and has been the source of countless issues for users over the years.

https://github.com/docker/compose/issues/745#issuecomment-346487757
