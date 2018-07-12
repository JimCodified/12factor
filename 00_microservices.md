# Microservices 

Twelve factor applications fall under the microservices architecture pattern. Microservices are described as architectural style characterized by several features. These features include:

* Services bounded by context
* Independent services
* Choreography vs orchestration
* Decentralized data management
* Infrastructure Automation / DevOps

## Service boundaries

Whether you are starting with a green field application or migrating a traditional application, determining what constitutes a microservice is the first task. Domain Driven Design (DDD) has been used as method of determining microservices. Domain Driven Design uses a model first approach to a service which defines a data model used by the service. Microservices represent atomic business functions that can be changed and deployed independently. Each microservice implements a single functional responsibility, which requires that business functions are decomposed into atomic functional services and data exchanges.

Components in a microservices architecture can be changed or replaced independently woth out affecting the other services in teh application. This means that a service owns the data and is responsible for its integrity. Microservices are independent runtime components where the data backing the service are also independent. In a bounded context, a strict definition would be a database or datastore per service. A common pattern in microservices is to use a key-value data store to back each service but it's also possible to use a relational database to back a service through the use of table based or schema based security that controls the interaction between the service and database.

Microservices are inherently a distributed computing platform and eventual consistency is part of that platform. However, some applications require ACID based transactions and the boundaries of the service API will need to respect them.

## Independent services

Modern software makes use of common libraries to implement their functions. In a microservices architecture, components are services that are independent of each other and replaceable. Services are out of process components that communicate with other services using web requests or remote procedure calls.

One reason for using services as components (rather than libraries) is that services are independently deployable. If an application is a single process, any changes to a component will require a redeployment of an application. However, if the application consists of multiple services, changes to a service will only affect that service while other services can remain running.

Independence of services enables rapid service development and deployment, and permits scalability through instantiation of parallel, independent services. This characteristic also provides resilience; a microsservice is allowed to fail and its responsibilities are taken over by parallel instantiations (of the same microservice), which do not depend on other services. When a microservice fails, it does not bring down other services.

Both design and runtime independence of services are required. It is necessary for the business to determine whether providing scalability and resilience of the business function are paramount considerations. If so, MSA provides a means of achieving these characteristics.

The voting application can be divided into a voting service, a worker service, and a results service. The voting service consists of a web application that writes to a Redis database to record votes. The results service uses PostgreSQL to store the votes and displays the tally of votes. Inbetween the voting and results service is a worker service that takes the votes registered in Redis and writes them to PostgreSQL.

## Choreography, smart endpoints and dumb pipes

Two methods of communication between services are orchestration and choreography. 

In orchestration the flow of data is controlled centrally or through a predefined set of instructions. When services uses orchestration they communicate via point-to-point connections between the services. In this scenario, one service calls the API of another service which results in predefined paths between all services. Integrating, changing or removing services is more difficult because each connection between services must be considered and addressed.

In choreography, each service responds to events as needed. One service doesn’t need to connect to another service in order to perform an action. Instead, each service observes events and acts on them autonomously. In the voting application it looks like this:

The voting service adds a vote to Redis and the worker service uses the Redis method BLPOP to pop the element from the head of the list. It the list it is empty, the connection is blocked until the next element is added to the list. The worker then writes the data to PostgreSQL. If we want to change the results database to MySQL we can make the change without stopping the voting service. This wouldn't be possible if the votes were written directly to the results database.

## Decentralized data management

Microservices decentralize data storage by letting each service manage it's own database. This can be different instances of the same database technology or a different database system, this approach is called Polyglot Persistence. Decentralizing data management among microservices changes how updates occur. In a monolith architecture, database transactions guarantee consistency when updating resources. While this promotes consistency, it also imposes a requirement for updates within a certain period of time. In a distributed system, this is problematic and difficult to implement. Microservices use transactionless updates between services with the understanding that consistency among the data stores will be eventual consistency.

## Infrastructure automation or DevOps

Martin Fowler noted that software development was typically project focused, i.e. the development team would work to deliver the software but their involvement stopped when the software was operational. In a DevOps model, the development and operations team work together and may be merged into a single team that works across the application life cycle. The hallmakr of DevOps is to automate processes that were tedious and slow using tooling to build application quickly and repeatability.

The use of containers to build and deploy the application and add certified components such as databases fits the DevOps model well. Building microservices with containers works well with the Continuous Integration/Continuous Deployment model, the toolchain to build and deploy the microservice are encapsulated in containers and the build, test and deploy processes are easily automated. This allows for small and frequent updates making deployments less risky. In addition to automating the development and test process, the voting application also adheres to the principal of infrastructure as code. Note that the application uses a Docker Compose file which defines how the application is deploy in a Docker Swarm, the one of the orchestrators supported by Docker

[Previous](README.md) - [Next](01_codebase.md)