# Docker Learning Journey

## Day 1-2 — Dockerfile Basics & Image Optimization

### What I built
A simple Python HTTP server containerized with Docker.

### Image size comparison
| Version | Base Image | Size |
|---------|-----------|------|
| v1 | ubuntu:22.04 | 183MB |
| v2 | python:3.11-slim | 124MB |
| v3 | python:3.11-alpine | 54MB |

### Key lessons
- RUN executes during build, CMD executes when container starts
- Docker caches layers — order Dockerfile least-to-most changing
- Base image choice has biggest impact on image size
- Container name vs Container ID vs Image name are different things

### Docker Hub
https://hub.docker.com/r/rakeshgowda54974/myapp
