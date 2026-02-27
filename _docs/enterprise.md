---
title: "Enterprise Features"
description: "Logging, health checks, and OpenAPI spec generation for production deployments"
---

### Logging

PSR-3-compatible JSON logger:

```php
$logger = new Logger(
    logDir: 'storage/logs',
    minLevel: 'info',  // Ignores debug messages
);

$logger->info('User logged in', ['user_id' => 42, 'ip' => '1.2.3.4']);
$logger->warning('Rate limit approaching', ['remaining' => 5]);
$logger->error('Payment failed', ['order_id' => 99, 'reason' => 'declined']);

// Message interpolation
$logger->info('User {user_id} performed {action}', ['user_id' => 42, 'action' => 'login']);
// Logs: "User 42 performed login"
```

Each log entry is a single JSON line in `storage/logs/app.log`:

```json
{"timestamp":"2026-02-27T14:30:00+00:00","level":"info","message":"User 42 performed login","context":{"user_id":42,"action":"login"}}
```

**Log levels** (in order of severity): `debug`, `info`, `notice`, `warning`, `error`, `critical`, `alert`, `emergency`. Setting `minLevel` filters out lower-severity messages.

### Health Checks

Monitor your application's dependencies:

```php
$health = new HealthCheck();

$health->addCheck('database', function () use ($db) {
    try {
        $db->fetchOne('SELECT 1');
        return ['status' => 'ok', 'message' => 'Connected'];
    } catch (\Throwable $e) {
        return ['status' => 'error', 'message' => $e->getMessage()];
    }
});

$health->addCheck('redis', function () use ($redis) {
    try {
        $redis->set('health', '1', ttl: 5);
        return ['status' => 'ok', 'message' => 'Connected'];
    } catch (\Throwable $e) {
        return ['status' => 'error', 'message' => $e->getMessage()];
    }
});

// Register as route
$router->get('/health', function () use ($health) {
    return $health->toResponse();
    // Returns 200 if all checks pass, 503 if any fail
});
```

Response format:

```json
{
  "status": "healthy",
  "checks": {
    "database": { "status": "ok", "message": "Connected", "latency_ms": 1.23 },
    "redis": { "status": "ok", "message": "Connected", "latency_ms": 0.45 }
  },
  "timestamp": "2026-02-27T14:30:00+00:00"
}
```

### OpenAPI Spec Generation

Auto-generate OpenAPI 3.0 documentation from your routes:

```php
$generator = new OpenApiGenerator(
    router: $app->router(),
    title: 'My API',
    version: '1.0.0',
    description: 'API documentation',
);

$router->get('/api/docs', function () use ($generator) {
    return Response::json($generator->generate());
});

// Or export as JSON string
file_put_contents('openapi.json', $generator->toJson());
```

Route parameters like `{id}` are automatically extracted as OpenAPI path parameters.
