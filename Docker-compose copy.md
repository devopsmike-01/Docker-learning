# Docker Compose and Docker Compose Commands

Docker Compose is a tool for defining and managing multi-container Docker applications. With a **YAML** file, you can specify `services`, `networks`, and `volumes`, allowing you to `deploy`, `test`, and `run` complex applications with a single command.

## What is Docker Compose?
- Docker Compose simplifies managing multi-container applications.
- It uses a YAML configuration file (docker-compose.yml) to define services, networks, and volumes.
- By using docker-compose up, all services are built and started as defined.

## Docker Compose Key Features

1. Multi-Container Orchestration:

- Define multiple services in a single configuration file.

**Example**: A `web` server, a `database`, and a `caching` layer.

2. Volume Management

- Persist data between container restarts.

3. Networking

- Automatically sets up communication between services.

4. Scalability

- Scale services up or down with a single command.

## Docker Compose Commands

Below are common Docker Compose commands, their explanations, and tailored examples:

1. Docker Compose Up

Starts and runs all services defined in the docker-compose.yml file.

**Command**:
```
docker-compose up
```

**Example**
```
docker-compose up -d
```

**Explanation**

- Starts all containers in detached mode `(-d)`, allowing the terminal to be free.
- Brings up services like `web`, `db`, and `cache` defined in `docker-compose.yml`.

*Use Case*

- Start a multi-service application like a WordPress site with a MySQL database.

2. Docker Compose Down

Stops and removes all `containers`, `networks`, and `volumes` created by `docker-compose up`.

**Command**

```
docker-compose down
```

**Example**

```
docker-compose down -v
```

**Explanation**

```
Stops containers and deletes associated volumes (-v).
```
*Use Case*

- Clean up resources after testing an application.

3. Docker Compose Build

Build or rebuild images for services defined in docker-compose.yml.

**Command**
```
docker-compose build
```

**Example**

```
docker-compose build web
```

**Explanation**

- Builds only the web service from the Compose file.

*Use Case*

- Rebuild a specific service after modifying its Dockerfile.

4. Docker Compose Logs

View logs from running containers managed by Docker Compose.

**Command**
```
docker-compose logs
```

**Example**
```
docker-compose logs -f
```
**Explanation**

Follows `(-f)` the real-time logs of all services.

*Use Case*

- Monitor the output of services during debugging or deployment.

5. Docker Compose Scale

Scale services to multiple container instances.

**Command**
```
docker-compose up --scale <service>=<number>
```

**Example**

```
docker-compose up --scale web=3
```
**Explanation**

- Scales the web service to 3 running containers.

**Use Case**

- Handle increased traffic by running multiple instances of a service.

6. Docker Compose Start

Starts existing stopped containers without recreating them.

**Command**
```
docker-compose start
```
**Example**
```
docker-compose start web db
```
**Explanation**

- Starts only the web and db services.

*Use Case*
- Resume previously stopped services.

7. Docker Compose Stop

Stops running containers without removing them.

**Command**
```
docker-compose stop
```
**Example**
```
docker-compose stop web
```
**Explanation**

- Stops the `web` service.

*Use Case*
- Temporarily stop services during maintenance.

8. Docker Compose Exec

Execute a command inside a running container.

**Command**
```
docker-compose exec <service> <command>
```
**Example**
```
docker-compose exec web ls /app
```
**Explanation**

Lists files in the `/app` directory inside the web service container.

*Use Case*
- Inspect or debug running containers.

9. Docker Compose Config

Validates and displays the final configuration.

**Command**
```
docker-compose config
```

**Explanation**

Checks the `YAML` syntax and outputs the merged configuration.

*Use Case*

- Ensure the Compose file is error-free before deployment.

10. Docker Compose Run

Run a one-off command in a service container.

**Command**
```
docker-compose run <service> <command>
```
**Example**

- `docker-compose run` web python `manage.py` migrate

**Explanation**
- Runs database migrations using the web service.

*Use Case*
- Execute administrative or setup tasks for a service.

## Practical Example: Docker Compose Application

Sample `docker-compose.yml` File

```
version: "3.8"
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./web:/usr/share/nginx/html
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
    volumes:
      - db_data:/var/lib/postgresql/data
volumes:
  db_data
```