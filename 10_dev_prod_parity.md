# 10 - Dev / Prod parity

The different environments must be as close as possible.

Docker is very good at reducing the gap as the same services can be deployed on the developer machine as they could on any Docker Hosts.

A lot of external services are available on Docker Hub and can be used in an existing application. Using those components enables a developer to use Postgres in development instead of SQLite or other lighter alternative. This reduces the risk of small differences that could show up later, when the app is on production. The application environment is consistent among development, testing and production.

This factor shows an orientation toward continuous deployment, where development can go from dev to production in a very short time frame, thus avoiding the big bang effect at each release.

## What does that mean for our application ?

The docker-compose file we built so far can be ran on your local machine or on any Docker host such as Docker Enterprise Edition. Developers can configure and application using a Docker Stack file or Kubernetes manifests. These same configuration files can be passed on to testing or production as the basis for a deployed application configuration. Common elements such as environmental variables,networks, volumes and secrets can be kept intact, while deployment configuration such as scaling, shutdown and startup, and runtime configurations can be changed according to need. The time needed to go from dev to production is significantly shortened.

The exact same application can run on either a development or production environment. Compare the [Docker Compose file](./example-voting-app/docker-compose.yml) used in development with the [Docker Stack file](./example-voting-app/docker-stack.yml) used in production.



[Previous](09_disposability.md) - [Next](11_logs.md)
