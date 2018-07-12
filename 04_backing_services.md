# 4 - External services

Handle external services as external resources of the application.

Examples:
* database
* log services
* ...

This ensures the application is loosely coupled with the services so it can easily switch provider or instance if needed

## What does that mean for our application ?

The voting application is a bundle of services. Each microservice plays its part to deploy the application. In addition to the services, the application uses two databases, Redis as a message queue and PostgreSQL for storage of results.

Both of these services are not part of the application code. Instead they are prebuilt containers that pull from the Docker Hub registry. The Docker Compose file pulls and configures these containers.

```yaml
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
```


[Previous](03_config.md) - [Next](05_build_release_run.md)
