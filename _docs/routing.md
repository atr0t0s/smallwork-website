---
title: "Routing"
description: "Define routes with parameters, groups, and per-route middleware in Smallwork"
---

Define routes in `config/routes/web.php` or `config/routes/api.php`. The router is passed as `$router`.

### Basic Routes

```php
$router->get('/hello', function (Request $request) {
    return Response::json(['message' => 'Hello, world!']);
});

$router->post('/users', function (Request $request) {
    $data = $request->json();
    return Response::json(['created' => $data['name']], 201);
});

$router->put('/users/{id}', function (Request $request) {
    $id = $request->param('id');
    return Response::json(['updated' => $id]);
});

$router->delete('/users/{id}', function (Request $request) {
    return Response::empty();
});
```

### Route Parameters

```php
$router->get('/posts/{slug}/comments/{id}', function (Request $request) {
    $slug = $request->param('slug');
    $id   = $request->param('id');
    return Response::json(compact('slug', 'id'));
});
```

### Route Groups

Groups share a URL prefix and optional middleware:

```php
$router->group('/api/v1', function (Router $r) {
    $r->get('/users', [UserController::class, 'index']);
    $r->post('/users', [UserController::class, 'store']);

    $r->group('/admin', function (Router $r) {
        $r->get('/stats', [AdminController::class, 'stats']);
    }, middleware: ['AdminOnly']);

}, middleware: ['AuthMiddleware']);
```

### Per-Route Middleware

```php
$router->get('/dashboard', [DashController::class, 'index'], middleware: ['AuthMiddleware']);
```
