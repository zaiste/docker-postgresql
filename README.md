Docker-PostgreSQL
=================

PostgreSQL in a Docker container (versions `9.2` and `9.3`).

Create Docker image
-------------------

For PostgreSQL `9.2`

```
cd docker-postgresql/9.2
docker build -t [your name]/postgresql:9.2 .
```

For PostgreSQL `9.3`

```
cd docker-postgresql/9.3
docker build -t [your name]/postgresql:9.3 .
```

First run
---------

By default, first container run initializes a database named `docker` with `docker` role and password. This can be overwritten by defining `POSTGRESQL_DB`, `POSTGRESQL_USER` and `POSTGRESQL_PASS` variables.

PostgreSQL server is configured to store data in `/data` inside the container. This directory can be mapped to any directory on the host using `-v` parameter, e.g. `/data` is mapped to `/tmp/data` on the host.

```
docker run -rm \
           -v /tmp/data:/data \
           -d \
           -e POSTGRESQL_DB="foo" \
           -e POSTGRESQL_USER="foo" \
           -e POSTGRESQL_PASS="foo" \
           -e PASS="$(pwgen -s -1 16)" \
           -t [your name]/postgresql:9.2
```

Connect to the container
------------------------

You must have `postgresql-client` installed on your host.

    apt-get install postgresql-client

Find the IP of the running container

    docker inspect -format="{{ .NetworkSettings.IPAddress }}" [Container ID]

And connect to it:

    psql -h 172.17.0.1 -p 5432 -U docker -W
