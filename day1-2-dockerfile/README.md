# Day 1-2 - Dockerfile Basics & Image Optimization

## What I Learned
How to write a Dockerfile from scratch and optimize image size.

## Image Size Comparison
| Version | Base Image | Size |
|---|---|---|
| v1 | ubuntu:22.04 | 183MB |
| v2 | python:3.11-slim | 124MB |
| v3 | python:3.11-alpine | 54MB |

## Dockerfile Instructions
FROM       → base image
RUN        → execute command during build
COPY       → copy files into image
WORKDIR    → set working directory
EXPOSE     → document port container listens on
CMD        → default command when container starts
ENTRYPOINT → fixed command that always runs

## Key Lessons
- RUN executes during build, CMD executes when container starts
- Docker caches layers — order Dockerfile least-to-most changing
- Base image choice has biggest impact on image size
- Copy requirements.txt before app code for layer cache efficiency
- Use slim or alpine variants in production

## Docker Hub
https://hub.docker.com/r/rakeshgowda54974/myapp
