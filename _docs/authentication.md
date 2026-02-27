---
title: "Authentication"
description: "JWT tokens, API keys, and auth middleware for protecting routes in Smallwork"
---

### JWT

```php
$jwt = new JwtAuth(secret: env('JWT_SECRET'));

// Create token
$token = $jwt->encode(
    payload: ['user_id' => 42, 'role' => 'admin'],
    expiresIn: 3600,
);

// Validate and decode
try {
    $payload = $jwt->decode($token);
    // ['user_id' => 42, 'role' => 'admin', 'iat' => ..., 'exp' => ...]
} catch (\RuntimeException $e) {
    // Invalid or expired token
}

// Refresh (issues new token with same payload)
$newToken = $jwt->refresh($oldToken, expiresIn: 3600);
```

### API Keys

```php
$apiKeys = new ApiKeyAuth($db);
$apiKeys->createTable();  // One-time setup

// Generate a new key
$result = $apiKeys->generate('My Service', permissions: ['chat:create', 'embeddings:read']);
// ['id' => 1, 'key' => 'sw_a1b2c3d4e5f6...']  (show key to user once)

// Verify incoming key
$info = $apiKeys->verify($key);
// ['id' => 1, 'name' => 'My Service', 'permissions' => [...], 'created_at' => '...']
// Returns null if invalid

// Manage keys
$apiKeys->revoke(1);
$all = $apiKeys->list();
```

### Auth Middleware

Protect routes with JWT or API key authentication:

```php
// JWT strategy
$authMiddleware = AuthMiddleware::jwt($jwt);

// API key strategy
$authMiddleware = AuthMiddleware::apiKey($apiKeys);

// Register
$app->container()->instance('auth', $authMiddleware);

$router->group('/api', function (Router $r) {
    $r->get('/profile', function (Request $request) {
        $user = $request->getAttribute('user');
        return Response::json($user);
    });
}, middleware: ['auth']);
```

The middleware reads `Authorization: Bearer <token>` or `X-Api-Key: <key>` headers and sets the `user` attribute on the request.
