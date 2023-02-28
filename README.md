# Go Backend Boilerplate

1. Create the db schema in [dbdiagram.io](https://dbdiagram.io/home).

   - [Example link](https://dbdiagram.io/d/63fd5c3f296d97641d842b75)
   - We can decouple the design from a specific database and click "Export" to
     export to the language you want. In this boilerplate, we choose Postgres.

1. Install Docker.

   ```bash
   # Install Docker
   brew install docker

   # Then run Docker app so that we can access the `docker` command.
   docker pull postgres:15-alpine

   # Check the downloaded image.
   docker images
   ```

   Then, run a container using the downloaded image. The command creates a new
   container named `postgres15` with PostgreSQL 15 running inside it. The
   container is configured with a user named `root` and a password of
   `password`. The container's port `5432` is mapped to port `5432` on the host
   machine, allowing it to be accessed from the host machine.

   ```bash
   docker run --name postgres15 \
      -p 5432:5432 \
      -e POSTGRES_USER=root \
      -e POSTGRES_PASSWORD=password \
      -d postgres:15-alpine
   ```

   ```bash
   # Enter the Postgres shell.
   docker exec -it postgres15 psql -U root

   # Try the following query in the shell.
   SELECT now();
   ```

   Install TablePlus.

   ```bash
   brew install tableplus
   ```

   Connect to Postgres with this setting

   ![](https://i.imgur.com/jgHY7h3.png)
