# 12 Factor Application Using Docker

The 12 factor methodology is the result of observations of applications deployed as services in the cloud. As the name states, it presents 12 principles that will help application to be cloud ready, horizontally scalable, and portable. The methodology includes:

* Automating setup using declarative formats to reduce time and cost for new developers
* Is portable across execution environments
* Built for the cloud
* Bring development and production closer together to enable continuous deployment
* Easily scales with changes to development, architecture or environment.
* Apps can be written in any programming language and use backing services such a databases, queues, and memory caches.

The 12 factor methodology enables building Software as a Service applications that are scalable and well structured.

## Organization of this lab

In this lab, we will use the voting application to follow each one of the 12 factors and see how this can leverage our application. At the same time, we will see that Docker is a really great fit for several of those factors.

## The Voting Application

This tutorial uses the voting application which records and displays votes between two different options. It is polyglot microservices application consisting of:

* Python webapp which lets you vote between two options
* Redis queue which collects new votes
* .NET or Java worker which consumes votes and stores them in
* Postgres database backed by a Docker volume
* Node.js webapp which shows the results of the voting in real time

## Let's get started !

[0 - Microservices](00_microservices.md)

[1 - Codebase](01_codebase.md)

[2 - Dependencies](02_dependencies.md)

[3 - Config](03_config.md)

[4 - Backing  services](04_backing_services.md)

[5 - Build / Release / Run](05_build_release_run.md)

[6 - Processes](06_processes.md)

[7 - Port binding](07_port_binding.md)

[8 - Concurrency](08_concurrency.md)

[9 - Disposability](09_disposability.md)

[10 - Dev / Prod parity](10_dev_prod_parity.md)

[11 - Logs](11_logs.md)

[12 - Admin processes](12_admin_processes.md)

Do not hesitate to provide comments / feedback you may have in order to improve this lab.
