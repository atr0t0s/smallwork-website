---
title: "Testing"
description: "Test helpers, TestCase base class, and AI mocking utilities in Smallwork"
---

### Test Helpers

Smallwork provides a base test class and AI mocking utilities:

```php
<?php
namespace Tests\Feature;

use Smallwork\Testing\TestCase;
use Smallwork\Testing\AIMock;
use Smallwork\Core\Response;

class ChatControllerTest extends TestCase
{
    public function test_homepage_returns_200(): void
    {
        $app = $this->createApp();
        $app->router()->get('/', fn() => Response::json(['ok' => true]));

        $response = $this->get('/');
        $this->assertEquals(200, $response->status());
    }

    public function test_create_post(): void
    {
        $app = $this->createApp();
        $app->router()->post('/posts', function ($request) {
            return Response::json($request->json(), 201);
        });

        $response = $this->json('POST', '/posts', ['title' => 'Hello']);
        $this->assertEquals(201, $response->status());
    }

    public function test_chat_endpoint_with_mocked_ai(): void
    {
        $gateway = AIMock::chat('Mocked AI response', [
            'prompt_tokens' => 10,
            'completion_tokens' => 20,
            'total_tokens' => 30,
        ]);

        $result = $gateway->chat([['role' => 'user', 'content' => 'Hello']]);
        $this->assertEquals('Mocked AI response', $result['content']);
    }

    public function test_embeddings_with_mocked_ai(): void
    {
        $gateway = AIMock::embed([[0.1, 0.2, 0.3]]);
        $vectors = $gateway->embed('test text');
        $this->assertEquals([0.1, 0.2, 0.3], $vectors[0]);
    }
}
```

**TestCase methods:**

| Method | Description |
|--------|-------------|
| `createApp()` | Creates a fresh App instance with test fixtures |
| `get(string $path)` | Sends a GET request, returns Response |
| `post(string $path, array $data)` | Sends a POST request with form data |
| `json(string $method, string $path, array $data)` | Sends a JSON request |

**AIMock methods:**

| Method | Description |
|--------|-------------|
| `AIMock::chat(string $content, array $usage)` | Returns Gateway that always responds with given content |
| `AIMock::embed(array $vectors)` | Returns Gateway that returns given embedding vectors |
