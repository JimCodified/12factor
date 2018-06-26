# 7 - Port binding

This factor is related to the exposition of the application to the outside.

To be compliant with 12 factor, an app must use specialized dependencies (such as http server, ...) and exposes its service through a port.

The host has the responsibility to route the request to the correct application through port mapping.

## What does that mean for our application ?

Docker already handles that for us, as we can see in the docker-compose file. Each  container exposes a port and in the case of services (vote and result) that communicate outside of the cluster, the host maps port 80 against their respective ports.

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

If several instances of the app services need to be deployed, the configuration above cannot be used. A given port on the host cannot map several ports in the containers.

In this case, we can deploy the application as s Docker stack file to scale the number of instances for each service. Docker EE will then orchestrate the traffic on the different instances of the services.

[Previous](06_processes.md) - [Next](08_concurrency.md)
