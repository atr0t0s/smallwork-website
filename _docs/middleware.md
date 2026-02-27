---
title: "Middleware"
description: "Writing and registering middleware, plus built-in CORS and rate limiting middleware"
---

Middleware follows the onion model. Each middleware receives the `Request` and a `$next` callable:

```php
class TimingMiddleware
{
    public function handle(Request $request, callable $next): Response
    {
        $start = microtime(true);
        $response = $next($request);
        $elapsed = round((microtime(true) - $start) * 1000, 2);
        return $response->withHeader('X-Response-Time', "{$elapsed}ms");
    }
}
```

### Registering Middleware

```php
// Global middleware (all routes)
$app->addMiddleware(new CorsMiddleware());
$app->addMiddleware(new RateLimitMiddleware(maxRequests: 100, windowSeconds: 60));

// Route group middleware
$router->group('/api', function (Router $r) {
    // routes here
}, middleware: ['AuthMiddleware']);

// Per-route middleware
$router->get('/admin', $handler, middleware: ['AdminOnly']);
```

### Built-in Middleware

**CORS**

```php
$cors = new CorsMiddleware(
    allowedOrigins: ['https://myapp.com', 'https://staging.myapp.com'],
    allowedMethods: ['GET', 'POST', 'PUT', 'DELETE'],
    allowedHeaders: ['Content-Type', 'Authorization'],
    maxAge: 86400,
);
$app->addMiddleware($cors);
```

**Rate Limiting**

```php
$limiter = new RateLimitMiddleware(
    maxRequests: 60,   // requests per window
    windowSeconds: 60, // window size
);
$app->addMiddleware($limiter);
// Returns 429 Too Many Requests when exceeded, with X-RateLimit-* headers
```

**Authentication** (see [Authentication](/docs/authentication/) section)

**Role/Permission** (see [Authorization](/docs/authorization/) section)
