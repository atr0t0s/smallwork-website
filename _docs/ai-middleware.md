---
title: "AI Middleware"
description: "Content moderation, intent classification, and auto-summarization middleware"
---

Add AI-powered processing to your request pipeline:

**Content Moderation** --- Block harmful content before it reaches your controllers:

```php
$moderation = new ContentModeration(
    gateway: $gateway,
    fields: ['message', 'content', 'text'],  // JSON fields to check
    provider: 'openai',
);
$app->addMiddleware($moderation);
// Returns 422 if content is classified as unsafe
```

**Intent Classification** --- Automatically classify user intent:

```php
$intent = new IntentClassifier(
    gateway: $gateway,
    categories: ['question', 'command', 'feedback', 'complaint', 'other'],
);
$app->addMiddleware($intent);

// In your controller
$router->post('/chat', function (Request $request) {
    $intent = $request->getAttribute('intent');  // e.g., "question"
    // Route to different handlers based on intent
});
```

**Auto-Summarizer** --- Summarize long inputs:

```php
$summarizer = new AutoSummarizer(
    gateway: $gateway,
    threshold: 500,  // Only summarize if text exceeds 500 chars
);
$app->addMiddleware($summarizer);

// In your controller
$router->post('/submit', function (Request $request) {
    $summary = $request->getAttribute('summary');
    // Short inputs: summary equals the original text (no API call)
    // Long inputs: AI-generated summary
});
```
