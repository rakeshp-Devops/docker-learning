cat > /home/rakeshp/docker-learning/day7-multicontainer/README.md << 'EOF'
# Day 7 - Multi-Container App + Docker Compose Full Notes

## Architecture Built Today
Browser → nginx (port 8080) → Flask (port 5000) → MySQL

nginx is the only service exposed to outside world.
Flask and MySQL are internal only - nobody talks to them directly.

---

## Docker Compose Full Revision Notes

### What is Docker Compose
Tool to define and run multi-container apps using a single yml file.
Instead of running 4 docker run commands - just run: docker compose up

### docker-compose.yml Structure
version: which compose format to use (use 3.9)
services: define all your containers here
volumes: declare named volumes used by any service

### Service Fields Explained

image: pull a pre-built image from Docker Hub
  example: image: mysql:8

build: build image from a Dockerfile in your project
  example: build: .  (Dockerfile in current folder)
  example: build: ./backend  (Dockerfile in backend folder)

ports: map host port to container port
  format: "host_port:container_port"
  example: "8080:80"  (access via localhost:8080, nginx listens on 80)
  only expose ports for services that need outside access

environment: pass env variables into container
  example:
    MYSQL_ROOT_PASSWORD: root
    MYSQL_DATABASE: myapp
    DB_HOST: db  (use service name as hostname - DNS magic)

depends_on: control startup order
  example: depends_on: - db  (start db before this service)
  with health check: depends_on: db: condition: service_healthy

volumes: mount named volumes or bind mounts
  named volume: - mysql_data:/var/lib/mysql
  bind mount:   - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf

healthcheck: check if service is ready before dependents start
  test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
  interval: 5s
  retries: 5

restart: always  (restart container if it crashes)

### Networks
Compose auto-creates a default network for all services.
All services can talk to each other by service name.
No need to define network manually for simple apps.
DNS works automatically - DB_HOST: db resolves to MySQL container IP.

### nginx Reverse Proxy Config
server {
    listen 80;
    location / {
        proxy_pass http://app:5000;
    }
}

listen 80: nginx listens on port 80 inside container
location /: match all incoming URL paths
proxy_pass: forward request to Flask service on port 5000
app:5000: app is the service name, Docker DNS resolves it

### Docker Compose Commands
docker compose up -d              # start all services in background
docker compose up --build -d      # rebuild images then start
docker compose down               # stop + remove containers + network
docker compose down -v            # also delete volumes (wipe data)
docker compose ps                 # list all services and status
docker compose logs               # view logs from all services
docker compose logs -f            # follow live logs
docker compose logs app           # logs from specific service
docker compose exec app bash      # shell into running container
docker compose restart app        # restart specific service

### Common Errors and Fixes
1. Container crashes immediately
   → run: docker compose logs <service>
   → read the error message carefully

2. MySQL crashes: database uninitialized
   → missing MYSQL_ROOT_PASSWORD environment variable

3. YAML errors: did not find expected key
   → indentation problem - all services need exactly 2 spaces
   → use cat > file << EOF to write files cleanly

4. Port already in use
   → something else is using that port on host
   → check with: lsof -i :80
   → change host port: "8080:80" instead of "80:80"

5. Volume not created (anonymous hash instead of named)
   → missing volumes: section at bottom of compose file
   → every named volume must be declared there

### build vs image - when to use which
image: use when pulling a standard image (mysql, nginx, redis)
build: use when you have custom code and a Dockerfile

### What docker compose down removes
- All containers
- The auto-created network
- Does NOT remove named volumes (data safe)
Use docker compose down -v to also remove volumes

---

## Files Created Today
docker-compose.yml  - 3 service definition
app.py              - Flask app
Dockerfile          - builds Flask image
requirements.txt    - flask dependency
nginx/nginx.conf    - reverse proxy config

## Real World Connection
Fullstack project (nginx + React + Node + MySQL) uses exact same pattern.
nginx sits in front, forwards /api to Node backend, serves React frontend.
Now I understand every line of that docker-compose.yml.
EOF
