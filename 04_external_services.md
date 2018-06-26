# 4 - External services

Handle external services as external resources of the application.

Examples:
* database
* log services
* ...

This ensure the application is loosely coupled with the services so it can easily switch provider or instance if needed

## What does that mean for our application ?

The voting application is a bundle of services. Each microservice plays it's part to deploy the application. In addition to the services, the application uses two databasees, redis as a queue and postgresql for storage.


[Previous](03_configuration.md) - [Next](05_build_ship_run.md)
