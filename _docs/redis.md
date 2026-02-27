---
title: "Redis"
description: "Using Redis for caching, sessions, and counters in Smallwork"
---

```php
// Production (real Redis via RESP protocol)
$redis = RedisAdapter::create([
    'host' => '127.0.0.1',
    'port' => 6379,
]);

// Testing (in-memory, no Redis needed)
$redis = RedisAdapter::createInMemory();

$redis->set('session:abc', json_encode($data), ttl: 3600);
$value = $redis->get('session:abc');
$redis->exists('session:abc');  // true
$redis->delete('session:abc');

$redis->increment('page:views');
$redis->decrement('stock:item:42');

$redis->flush();  // Clear everything
```
