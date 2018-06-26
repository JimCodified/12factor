# 12 Factor Application Using Docker

The 12 factor methodology is the result of observations of applications deployed as services in the cloud. As the name states, it presents 12 principles that will help application to be cloud ready, horizontally scalable, and portable.

# Organization of this lab

In this lab, we will start by building a simple Node.js application as an HTTP Rest API exposing CRUD (Create / Read / Update / Delete) verbs on a *message* model.

HTTP verb | URI | Action
----------| --- | ------
GET | /message | list all messages
GET | /message/ID | get message with ID
POST | /message | create a message user
PUT | /message/ID | modify message with ID
DELETE | /message/ID | delete message with ID

We will then follow each one of the 12 factor and see how this can leverage our application. At the same time, we will see that Docker is a really great fit for several of those factors.

# Let's get started !

[Build the application](00_application.md)

[1 - Codebase](01_codebase.md)

[2 - Dependencies](02_dependencies.md)

[3 - Configuration](03_configuration.md)

[4 - External services](04_external_services.md)

[5 - Build / Ship / Run](05_build_ship_run.md)

[6 - Processes](06_processes.md)

[7 - Port binding](07_port_binding.md)

[8 - Concurrency](08_concurrency.md)

[9 - Disposability](09_disposability.md)

[10 - Dev / Prod parity](10_dev_prod_parity.md)

[11 - Logs](11_logs.md)

[12 - Admin processes](12_admin_processes.md)

Do not hesitate to provide comments / feedback you may have in order to improve this lab.
