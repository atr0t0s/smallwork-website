---
title: "HTMX Integration"
description: "Building interactive UIs with HTMX helpers and JSON API mode in Smallwork"
---

Build interactive UIs without JavaScript using HTMX:

```php
// Check if request came from HTMX
if (HtmxHelper::isHtmxRequest($request)) {
    return HtmxHelper::partial('<li>New item added</li>');
}

// Trigger client-side events
$response = HtmxHelper::partial('<p>Saved!</p>');
$response = HtmxHelper::trigger($response, 'itemAdded');
$response = HtmxHelper::trigger($response, ['itemAdded', 'refreshList']);

// Navigation
$response = HtmxHelper::redirect('/dashboard');
$response = HtmxHelper::refresh();

// Retarget the swap
$response = HtmxHelper::retarget($response, '#notifications');
$response = HtmxHelper::reswap($response, 'beforeend');
$response = HtmxHelper::pushUrl($response, '/items/42');
```

### JSON API Mode

For React/Vue/Angular/Svelte apps, use the JSON API --- no special integration needed:

```php
$router->group('/api/v1', function (Router $r) {
    $r->get('/posts', function (Request $request) {
        return Response::json(['posts' => $posts]);
    });
});
```

Configure CORS for your SPA origin and consume the API from any frontend framework.
