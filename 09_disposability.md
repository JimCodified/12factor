# 9 - Disposability

Each process of an application must be disposable.

* it must have a quick startup
  * ease the horizontal scalability
* it must ensure a clean shutdown
  * stop listening on the port
  * finish to handle the current request
  * usage of a queueing system for long lasting (worker type) process

## What does that mean for our application ?

Our application exposes HTTP endPoints that are easy and quick to handle. Having multiple clients writing to the PostgreSQL database can cause a bottleneck where votes can be lost due to a long running process that could be interrupted . The voting application uses a message queue to store votes from the voting clients before writing them into the database where the results can be tallied. Each time a vote is entered, the worker pushes the last vote on the list until al votes are entered.

[Previous](08_concurrency.md) - [Next](10_dev_prod_parity.md)
