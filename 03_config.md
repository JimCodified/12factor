# 3 - Configuration

Configuration (credentials, database connection string, ...) should be stored in the environment.

## What does that mean for our application ?

In _Worker.java_, we define the _redis_ and _dbConn_ connections and use the service name defined in the Docker Compose file.

```java
class Worker {
  public static void main(String[] args) {
    try {
      Jedis redis = connectToRedis("redis");
      Connection dbConn = connectToDB("db");
```

In _app.py_, we make sure the _redis_ connection defined as a service in the Compose file is used.

```python
def get_redis():
    if not hasattr(g, 'redis'):
        g.redis = Redis(host="redis", db=0, socket_timeout=5)
    return g.redis
```

In the result service, _server.js_ sets the database connection _DB_HOST_ in the Compose file as environment variable.  

```js
const databaseHost = process.env.DB_HOST;
var port = process.env.PORT || 4000;

async.retry(
  {times: 1000, interval: 1000},
  function(callback) {
    pg.connect(DB_HOST, function(err, client, done) {
      if (err) {
        console.error("Waiting for db");
      }
      callback(err, client);
    });
  },
  function(err, client) {
    if (err) {
      return console.error("Giving up");
    }
    console.log("Connected to db");
    getVotes(client);
  }
);

```

When deploying services in Docker we can can use the name of a service in a Docker Compose file to route requests between services. We can also define environmental variables in a Compose file and call them in the code.

[Previous](02_dependencies.md) - [Next ](04_backing_services.md)
