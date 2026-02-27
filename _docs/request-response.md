---
title: "Request & Response"
description: "Working with HTTP requests and building responses in Smallwork"
---

## Request

The `Request` object wraps all HTTP input:

```php
// Query string: /search?q=php&page=2
$query = $request->query('q');          // "php"
$page  = $request->query('page', 1);   // "2"
$all   = $request->query();            // ['q' => 'php', 'page' => '2']

// POST / form data
$name = $request->input('name');

// JSON body (auto-parsed)
$data  = $request->json();             // Full decoded array
$email = $request->json('email');      // Specific key

// Headers (case-insensitive)
$token = $request->header('Authorization');
$all   = $request->headers();

// Route parameters
$id = $request->param('id');

// Raw body
$raw = $request->rawBody();

// Method helpers
$request->isGet();
$request->isPost();

// Custom attributes (set by middleware)
$user = $request->getAttribute('user');
```

## Response

Build responses with static factory methods:

```php
// JSON
return Response::json(['users' => $users]);
return Response::json(['error' => 'Not found'], 404);

// HTML
return Response::html('<h1>Hello</h1>');

// Empty (204 No Content)
return Response::empty();

// Redirect
return Response::redirect('/login');
return Response::redirect('/new-location', 301);

// Server-Sent Events (SSE) streaming
return Response::stream(function () {
    echo "data: chunk 1\n\n";
    flush();
    echo "data: chunk 2\n\n";
    flush();
});
```

Responses are immutable --- `withHeader` and `withCookie` return new instances:

```php
return Response::json($data)
    ->withHeader('X-Request-Id', $requestId)
    ->withHeader('Cache-Control', 'no-store')
    ->withCookie('session', $token, maxAge: 3600);
```
