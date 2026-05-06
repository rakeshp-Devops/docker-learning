# Day 6 - Docker Compose

## What is Docker Compose
Docker Compose lets you define and run multi-container applications
using a single docker-compose.yml file instead of running multiple
docker run commands manually.

## docker-compose.yml structure
version: which compose format to use
services: define your containers here
volumes: declare named volumes used by services

## Fields I learned
- image: use a pre-built image from Docker Hub
- build: build image from a Dockerfile
- ports: map host port to container port (host:container)
- environment: pass environment variables into container
- depends_on: control startup order between services
- volumes: mount named volumes or bind mounts

## Commands
docker compose up -d        # start all services in background
docker compose up --build   # rebuild images before starting
docker compose down         # stop and remove containers and network
docker compose ps           # list running services
docker compose logs app     # view logs of a specific service

## What I built
2-service app from scratch:
- Flask app (built from Dockerfile) running on port 5000
- MySQL database with named volume for data persistence
- Flask waits for MySQL via depends_on

## Errors I fixed
- MySQL crashed: missing MYSQL_ROOT_PASSWORD environment variable
- Flask crashed: wrong import (Flask vs flask), indentation errors
- Volume not created: missing volumes section at bottom of compose file

## Real World Connection
My fullstack project (nginx + React + Node + MySQL) uses Docker Compose.
Now I understand every line of that docker-compose.yml file.
Compose auto-creates a network so all services talk by container name.
