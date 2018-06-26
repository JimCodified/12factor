# 6 - Processes

An application is made up of several processes.

Each process must be stateless and must not have local storage (sessions, ...).

This is required
* for scalability
* fault tolerance (crashes, ...)

The data that need to be persisted, must be saved in a stateful resources (Database, shared filesystem, ...)

Eg: sessions can easily be saved in a Redis kv store

Note: Sticky session violate 12 factor.

## What does that mean for our application ?

The voting application stores session in a distributed Redis kv store. (MongoDB is another possible option).


```python
    if request.method == 'POST':
        redis = get_redis()
        vote = request.form['vote']
        data = json.dumps({'voter_id': voter_id, 'vote': vote})
        redis.rpush('votes', data
```

Redis is included in the Docker compose file

```yaml
  redis:
    image: redis:alpine
    container_name: redis
    ports: ["6379"]
    networks:
      - back-tie
```

[Previous](05_build_ship_run.md) - [Next](07_port_binding.md)
