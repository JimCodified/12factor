# 4 - External services

Handle external services as external resources of the application.

Examples:
* database
* log services
* ...

This ensures the application is loosely coupled with the services so it can easily switch provider or instance if needed

## What does that mean for our application ?

The voting application is a bundle of services. Each microservice plays its part to deploy the application. In addition to the services, the application uses two databases, Redis as a message queue and PostgreSQL for storage of results.


[Previous](03_configuration.md) - [Next](05_build_ship_run.md)
