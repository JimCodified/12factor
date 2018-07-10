# 8 - Concurrency

Horizontal scalability with the processes model.

The app can be seen as a set of processes of different types
* web server
* worker
* message queue

Each process needs to be able to scale horizontally, it can have its own internal multiplexing.

## What does that mean for our application ?

The  voting app has several processes that include a web UI for voting that writes to the message queue in Redis, a worker that reads the data from Redis and writes to PostgreSQL, and client that displays the results.

If we want to scale the application, we can deploy the application as a Docker stack file to increase the number of instances for each service. Docker EE will then orchestrate the traffic on the different instances of the services. The processes are easily scalable because they are stateless processes. Note the `replicas` element which sets the number of containers to deploy for that service.

```yaml
version: "3"
services:

  redis:
    image: redis:alpine
    ports:
      - "6379"
    networks:
      - frontend
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure
  db:
    image: postgres:9.4
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    deploy:
      placement:
        constraints: [node.role == manager]
  vote:
    image: dockersamples/examplevotingapp_vote:before
    ports:
      - 5000:80
    networks:
      - frontend
    depends_on:
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure
  result:
    image: dockersamples/examplevotingapp_result:before
    environment:
      - DB_HOST=postgres://postgres@db/postgres
    ports:
      - 5001:80
    networks:
      - backend
    depends_on:
      - db
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  worker:
    image: dockersamples/examplevotingapp_worker
    networks:
      - frontend
      - backend
    deploy:
      mode: replicated
      replicas: 1
      labels: [APP=VOTING]
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints: [node.role == manager]

  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    stop_grace_period: 1m30s
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]

networks:
  frontend:
  backend:

volumes:
  db-data:

```

[Previous](07_port_binding.md) - [Next](09_disposability.md)
