# Docker / Best practices

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
      dockerfile: .docker/bundle.Dockerfile
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
