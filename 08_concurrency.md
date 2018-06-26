# 8 - Concurrency

Horizontal scalability with the processes model.

The app can be seen as a set of processes of different types
* web server
* worker
* message queue

Each process needs to be able to scale horizontally, it can have its own internal multiplexing.

## What does that mean for our application ?

The  voting app has several processes that include a web UI for voting that writes to the message queue in Redis, a worker that reads the data from Redis and writes to PostgreSQL, and client that displays the results.

If we want to scale the application, we can deploy the application as s Docker stack file to increase the number of instances for each service. Docker EE will then orchestrate the traffic on the different instances of the services. The processes are easily scalable because they are stateless processes.

[Previous](07_port_binding.md) - [Next](09_disposability.md)
