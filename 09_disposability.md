# 9 - Disposability

Each process of an application must be disposable.

* it must have a quick startup
  * ease the horizontal scalability
* it must ensure a clean shutdown
  * stop listening on the port
  * finish to handle the current request
  * usage of a queuing system for long lasting (worker type) process

## What does that mean for our application ?

Our application exposes HTTP endPoints that are easy and quick to handle. Having multiple clients writing to the PostgreSQL database can cause a bottleneck where votes can be lost due to a long running process that could be interrupted . The voting application uses a message queue to store votes from the voting clients before writing them into the database where the results can be tallied. Each time a vote is entered, the worker pushes the last vote on the list until al votes are entered.

With the Docker Enterprise Edition, you have a choice in deploying an application using either Docker Swarm or Kubernetes according to your needs. Docker EE will create and destroy containers with services as configured in either a Docker Stack file or Kubernetes manifests. Swarm or Kubernetes can be used to scale applications by increasing the number of running containers. Changes to the application, such as deploying an updated service, are done by either Docker Swarm or Kubernetes through rolling updates.

Let's look at the service entry for the voting service:

```yaml
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
```
Under `deploy`, the service has two instances running, `update_config` sets the number of containers to update at a time, and `restart_policy` sets how to restart containers when they exit.

The Kubernetes deployment manifest is similarly configured.

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: vote
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: vote
    spec:
      containers:
      - image: dockersamples/examplevotingapp_vote:before
        name: vote
```

[Previous](08_concurrency.md) - [Next](10_dev_prod_parity.md)
