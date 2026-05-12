# Day 8 - Docker Debugging

## Four Debugging Tools
1. docker compose logs - see container output
2. docker compose exec - get inside running container
3. docker inspect - full container details
4. docker stats - live CPU and memory usage

## Commands
docker compose logs app          # logs from specific service
docker compose logs -f           # follow live logs
docker compose exec app sh       # shell into container
docker inspect <container>       # full details
docker inspect <container> | grep "ExitCode"
docker inspect <container> | grep -A 5 "IPAddress"
docker stats --no-stream         # one snapshot

## Exit Codes
0   → clean exit
1   → app error
137 → killed by Docker (SIGKILL after 10s timeout)
130 → Ctrl+C

## Key inspect fields
ExitCode     → why container stopped
OOMKilled    → killed due to out of memory
RestartCount → how many times Docker restarted
IPAddress    → container IP
DNSNames     → all names Docker registered

## Debugging Workflow
1. docker compose ps          → is it running?
2. docker compose logs app    → what did it print?
3. docker inspect             → exit code? OOMKilled?
4. docker stats               → CPU/memory issue?
5. docker compose exec app sh → investigate inside

## What I practiced
- Stopped Flask → saw 502 Bad Gateway from nginx
- Started Flask back → site recovered
- Checked exit code 137 after docker compose stop
- Used docker stats to see CPU and memory per container
- Used docker exec to get inside container and run env/ls
