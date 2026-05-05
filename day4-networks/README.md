# Day 4 - Docker Networks

## What I Learned
Docker networks allow containers to communicate with each other.
Without a network, containers are isolated and cannot talk to each other.

## Network Types
- bridge: containers on same network talk by name. Most common.
- host: container shares host machine network directly
- none: completely isolated, no network

## Commands Used

# Create a network
docker network create mynetwork

# Run container on a network
docker run -d --name container1 --network mynetwork alpine sleep 3000

# Ping container by name
docker exec container2 ping container1 -c 3

# Inspect network - see connected containers and IPs
docker network inspect mynetwork

# Connect container to additional network
docker network connect mynetwork container3

# Remove network
docker network rm mynetwork

## Real World Connection
In my fullstack project, nginx, Node and MySQL all communicate
by container name because they are on the same custom bridge network.
Docker Compose sets this up automatically.
