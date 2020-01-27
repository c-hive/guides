# Docker / Services

Each preferably with minimal image and data volume.

```yml
mysql:
  image: mysql
  volumes:
    - /var/lib/mysql

postgres:
  image: postgres:alpine
  volumes:
    - /var/lib/postgresql/data

redis:
  image: redis:alpine
  volumes:
    - /data
```
