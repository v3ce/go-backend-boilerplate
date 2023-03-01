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

1. Install
   [`golang-migrate/migrate`](https://github.com/golang-migrate/migrate).

   ```bash
   # Install migrate.
   brew install golang-migrate

   # Check the installed migrate command.
   migrate --version

   # Create the db migration directory.
   mkdir -p db/migration

   # Create the first migration script.
   migrate create -ext sql -dir db/migration -seq init_schema
   ```

   Now, create a [Makefile](./Makefile) to save time and run the following:

   ```bash
   # Run a postgres container.
   make postgres

   # Create a db called "simple_bank" in this tutorial.
   make createdb

   # Migrate to create tables in the db.
   make migrateup
   ```

1. Install [`sqlc`](https://github.com/kyleconroy/sqlc).

   ```bash
   # Install sqlc.
   brew install sqlc

   # Check the installed sqlc.
   sqlc version
   ```

   Initialize [`sqlc.yaml`](./sqlc.yaml) and copy the initial config from
   [Getting started with PostgreSQL](https://docs.sqlc.dev/en/stable/tutorials/getting-started-postgresql.html#getting-started-with-postgresql)
   with some modifications.

   ```bash
   sqlc init
   ```

   Add the queries in [account.sql](./db/query/account.sql), then
   `sqlc generate` to codegen.

   ```bash
   # Codegen.
   make sqlc

   # Eliminate red lines inside `db/sqlc/account.sql`.
   go mod init github.com/v3ce/go-backend-boilerplate
   ```

1. To write unit tests, we need to connect to the DB driver and import it in
   [main_test.go](./db/sqlc/main_test.go).

   ```bash
   # Get required driver.
   go get github.com/lib/pq

   # Remove "indirect" of lib/pq.
   go mod tidy
   ```
