# Docker / Best practices

#### Combine non-hanging entrypoints with docker-compose commands

When using `docker-compose` use `ENTRYPOINT` to ensure that the container is ready to use and use `command` to start the process. This way the entrypoint doesn't hang and it's possibly to also `run` other commands by hand.

`Dockerfile`
```Dockerfile
COPY . ./
RUN chmod +x .docker/web-entrypoint.sh
ENTRYPOINT [".docker/web-entrypoint.sh"]
```

`.docker/web-entrypoint.sh`
```sh
wait_for-it db:5432
exec "$@"
```

`docker-compose.yml`
```sh
services:
  web:
    command: 'bin/rails server'
```

Note that
- if using `ENTRYPOINT .docker/web-entrypoint.sh`, `exec "$@"` won't work, instead use `ENTRYPOINT [".docker/web-entrypoint.sh"]`
- if `bin/rails server` would be in `web-entrypoint.sh`, executing commands via `docker-compose run` would not be possible

#### Do not put 1-time setup in entrypoint

It would run every time.

GOOD

`.docker/web-entrypoint.sh`
```sh
wait_for-it db:5432
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
version: '3.7'
services:

  web:
    build:
      context: ../
      dockerfile: .docker/Dockerfile
```

#### Use separate files for separate environments and services when needed

```yml
# .docker/docker-compose.dev.yml
version: '3.7'
services:

  web:
    build:
      context: ../
      dockerfile: .docker/web.dev.Dockerfile
  
  cache:
    build:
      context: ../
      dockerfile: .docker/cache.dev.Dockerfile
```

#### Use separate service to cache dependencies in development environment

```yml
# .docker/docker-compose.dev.yml
version: '3.7'
services:

  web:
    build: .
    command: 'bash -c "wait-for-it cache:1337 && bin/rails server"'
    depends_on:
      - cache
    volumes:
      - cache:/bundle
    environment:
      BUNDLE_PATH: '/bundle'

  cache:
    build:
      context: ../
      dockerfile: .docker/cache.Dockerfile
    volumes:
      - bundle:/bundle
    environment:
      BUNDLE_PATH: '/bundle'
    ports:
      - "1337:1337"

volumes:
  cache:
```

```Dockerfile
# .docker/cache.Dockerfile
FROM ruby:2.6.3
RUN apt-get update -qq && apt-get install -y netcat-openbsd
COPY Gemfile* ./
COPY .docker/cache-entrypoint.sh ./
RUN chmod +x cache-entrypoint.sh
ENTRYPOINT ./cache-entrypoint.sh
```

```bash
# .docker/cache-entrypoint.sh
#!/bin/bash

bundle check || bundle install
nc -l -k -p 1337
```

```Dockerfile
# web.dev.Dockerfile
FROM ruby:2.6.3
RUN apt-get update -qq && apt-get install -y nodejs wait-for-it
WORKDIR ${GITHUB_WORKSPACE:-/app}
# Note: bundle install step removed
COPY . ./
```
