# 7 - Port binding

This factor exposes the application outside of the cluster, making it available publicly.

To be compliant with 12 factor, an app must use specialized dependencies (such as http server, ...) and exposes its service through a port.

The host has the responsibility to route the request to the correct application through port mapping.

## What does that mean for our application ?

Docker handles that for us in the docker-compose file. Each  container exposes a port and in the case of services (vote and result) that communicate outside of the cluster, the host maps port 80 to other external ports because two services can not share the same port.

```yaml
version: "3"

services:
  vote:
    build: ./vote
    command: python app.py
    volumes:
     - ./vote:/app
    ports:
      - "5000:80"
    networks:
      - front-tier
      - back-tier

  result:
    build: ./result
    command: nodemon server.js
    environment:
      - DB_HOST=postgres://postgres@db/postgres
      - PORT=4000
    volumes:
      - ./result:/app
    ports:
      - "5001:80"
      - "5858:5858"
    networks:
      - front-tier
      - back-tier

  worker:
    build:
      context: ./worker
      dockerfile: Dockerfile.j
    networks:
      - back-tier

  redis:
    image: redis:alpine
    container_name: redis
    ports: ["6379"]
    networks:
      - back-tier

  db:
    image: postgres:9.4
    container_name: db
    volumes:
      - "db-data:/var/lib/postgresql/data"
    networks:
      - back-tier

volumes:
  db-data:

networks:
  front-tier:
  back-tier:
```

[Previous](06_processes.md) - [Next](08_concurrency.md)
