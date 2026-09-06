# Popeye

Epitech project focused on containerizing and orchestrating a web poll application using Docker and Docker Compose.

## Description

Popeye is designed to practice the fundamentals of application containerization and multi-container infrastructure description with Docker and Docker Compose.

The deployed application is a voting system made of five components:

- **Poll**: Flask (Python) web application that collects votes and pushes them into a Redis queue.
- **Redis**: queue that temporarily holds the votes sent by Poll, waiting to be processed by the Worker.
- **Worker**: Java application that consumes the votes from Redis and stores them into a PostgreSQL database.
- **PostgreSQL**: database providing persistent storage for the votes.
- **Result**: Node.js web application that fetches the votes from the database and displays the results.

## Architecture

```
Poll (Flask)          Result (Node.js)
     |                       |
     v                       v
  Redis  <----------------> PostgreSQL
     ^                       ^
     |                       |
     +------- Worker (Java) -+
```

## Project structure

```
.
|-- docker-compose.yml
|-- schema.sql
|-- poll
|   |-- Dockerfile
|-- result
|   |-- Dockerfile
\-- worker
    \-- Dockerfile
```

## Requirements

- Docker
- Docker Compose

## Configuration

Services are configured through environment variables defined in the `docker-compose.yml` file:

| Variable | Description | Used by |
|---|---|---|
| `REDIS_HOST` | Hostname of the Redis service | Poll, Worker |
| `POSTGRES_HOST` | Hostname of the database service | Result, Worker |
| `POSTGRES_PORT` | Listening port of PostgreSQL | Result, Worker |
| `POSTGRES_DB` | Name of the database | Result, Worker |
| `POSTGRES_USER` | User used to connect to the database | Result, Worker |
| `POSTGRES_PASSWORD` | Password used to connect to the database | Result, Worker |

The database password is never stored in plain text in the repository. It is provided through a local `.env` file, excluded from version control via `.gitignore`.

## Running the project

```bash
docker compose up --build
```

Services are then accessible at the following addresses:

- Poll: http://localhost:5000
- Result: http://localhost:5001

A vote submitted on the Poll page should be reflected on the Result page.

Containers restart automatically if they stop unexpectedly.

## Networks

Three networks isolate communication between services:

- `poll-tier`: communication between Poll and Redis.
- `result-tier`: communication between Result and the database.
- `back-tier`: communication between Worker, Redis, and the database.

## Data persistence

A named volume, `db-data`, ensures PostgreSQL data persistence independently of the container lifecycle.

## Security

No password or secret is stored in plain text in the repository. The `POSTGRES_PASSWORD` variable is provided at runtime through a non-versioned environment file.

## Author

Project developed as part of the Epitech curriculum.
