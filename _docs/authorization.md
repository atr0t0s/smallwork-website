---
title: "Authorization"
description: "Role-based access control (RBAC) and permission middleware in Smallwork"
---

### Role-Based Access Control (RBAC)

Configure roles and permissions in `config/auth.php`:

```php
return [
    'roles' => [
        'admin'   => ['chat:create', 'chat:read', 'users:manage', 'embeddings:write'],
        'user'    => ['chat:create', 'chat:read'],
        'service' => ['embeddings:write', 'embeddings:read'],
    ],
];
```

Use in code:

```php
$roles = new RoleManager($config['roles']);

$roles->hasPermission('admin', 'users:manage');  // true
$roles->hasPermission('user', 'users:manage');   // false
$roles->getPermissions('admin');                  // ['chat:create', ...]
$roles->roleExists('admin');                      // true
```

### Role and Permission Middleware

```php
// Require a specific role
$adminOnly = new RoleMiddleware($roles, 'admin');

// Require a specific permission
$canManage = RoleMiddleware::requirePermission($roles, 'users:manage');

// Apply to routes
$router->group('/admin', function (Router $r) {
    $r->get('/users', [AdminController::class, 'users']);
}, middleware: [$adminOnly]);
```

The middleware reads `$request->getAttribute('user')['role']` (set by AuthMiddleware) and returns 403 if unauthorized.
