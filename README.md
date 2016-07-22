catbox-redis
============

Redis adapter for catbox (this fork is using ioredis instead of node_redis and therefore makes connecting via Redis Sentinel possible)

Lead Maintainer of original catbox-redis (https://github.com/hapijs/catbox-redis): [Loic Mahieu](https://github.com/LoicMahieu)

## Options

- `name` - identifies a group of Redis instances composed of a master and one or more slaves (only used with `sentinels` option, if `name` and `sentinels` are provided, `host`, `port` and `socket` are ignored)
- `sentinels` - are a list of sentinels to connect to. The list does not need to enumerate all your sentinel instances, but a few so that if one is down the client will try the next one (only used with `name` option, if `name` and `sentinels` are provided, `host`, `port` and `socket` are ignored)
- `host` - the Redis server hostname. Defaults to `'127.0.0.1'`.
- `port` - the Redis server port or unix domain socket path. Defaults to `6379`.
- `socket` - the unix socket string to connect to (if `socket` is provided, `host` and `port` are ignored)
- `password` - the Redis authentication password when required.
- `database` or `db` - the Redis database.
- `partition` - this will store items under keys that start with this value. (Default: '')

## Tests

The test suite expects:
- a redis server to be running on port 6379
- a redis server listening to port 6378 and requiring a password: 'secret'
- a redis server listening on socket `/tmp/redis.sock`
- a redis-sentinel server listening on port 26378 using redis server running on port 6378
- a redis-sentinel server listening on port 26379 using redis server running on port 6379

See [.travis.yml](./.travis.yml)

```sh
redis-server &
redis-server --port 6378 --requirepass secret &
redis-server --port 6377 --unixsocket /tmp/redis.sock &
redis-sentinel test/sentinel.conf &
redis-sentinel test/sentinel2.conf &
npm test
```
