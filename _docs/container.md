---
title: "Dependency Injection Container"
description: "Using the DI container for binding, singletons, instances, and auto-wiring in Smallwork"
---

The container supports binding, singletons, instances, and auto-wiring:

```php
$container = $app->container();

// Bind a factory (new instance each time)
$container->bind(Mailer::class, fn() => new Mailer(env('SMTP_HOST')));

// Singleton (created once, reused)
$container->singleton(Gateway::class, function () {
    $gw = new Gateway('openai');
    $gw->register('openai', new OpenAIProvider(
        baseUrl: 'https://api.openai.com/v1',
        apiKey: env('OPENAI_API_KEY'),
    ));
    return $gw;
});

// Register an existing instance
$container->instance('config', $configArray);

// Resolve
$mailer = $container->resolve(Mailer::class);

// Auto-wire (resolves constructor dependencies via type hints)
$controller = $container->make(UserController::class);
```
