+++
date = "2017-04-15T14:39:04+02:00"
title = "Example using Postgres"
url = "postgres-example"

[menu.usage]
  Parent = "examples"
  weight = 5
  identifier = "postgres-example"
+++

Example Yaml configuration for a project with a Postgres service dependency. Note that the postgres service will be accessiable at `database:5432`.

```yaml
pipeline:
  test:
    image: golang
    commands:
      - go get
      - go test

services:
  database:
    image: postgres
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_DB=test
```

The official Postgres image provides environment variables used at startup to create the default username, password, database and more. Please see the official image documentation for more details.

```diff
services:
  database:
    image: postgres
    environment:
+     - POSTGRES_USER=postgres
+     - POSTGRES_DB=test
```

If you are unable to connect to the Postgres container please make sure you are giving Postgres adequate time to initialize and begin accepting connections.

```diff
pipeline:
  test:
    image: golang
    commands:
+     - sleep 15
      - go get
      - go test

services:
  database:
    image: postgres
```

If you are still unable to connect to the mysql container, please make sure you using container name as the address.

```diff
- psql -U root -d test -h 127.0.0.1:5432
+ psql -U root -d test -h tcp://database:5432
```