---
title: "Chat Completions"
description: "AI chat completions, multi-turn conversations, and real-time streaming with SSE"
---

### Chat Completions

```php
// One-shot
$result = $gateway->chat([
    ['role' => 'user', 'content' => 'Explain PHP in one sentence.'],
]);
echo $result['content'];
// $result['usage'] = ['prompt_tokens' => ..., 'completion_tokens' => ..., 'total_tokens' => ...]

// Choose provider per request
$result = $gateway->chat($messages, provider: 'anthropic');

// With options
$result = $gateway->chat($messages, options: [
    'temperature' => 0.7,
    'max_tokens'  => 500,
    'model'       => 'gpt-4o-mini',
]);
```

### Conversations (Chat Service)

Manage multi-turn conversations with history tracking:

```php
$chat = new Chat(
    gateway: $gateway,
    systemPrompt: 'You are a helpful coding assistant.',
    provider: 'openai',
    options: ['temperature' => 0.7],
);

$response1 = $chat->send('What is dependency injection?');
echo $response1['content'];

$response2 = $chat->send('Show me an example in PHP.');
echo $response2['content'];
// History is maintained — the AI sees both messages

// Streaming
$chat->stream('Explain closures.', function (string $chunk) {
    echo $chunk;
    flush();
});

// Track token usage across the conversation
$usage = $chat->getTotalUsage();
// ['prompt_tokens' => 450, 'completion_tokens' => 320, 'total_tokens' => 770]

// Inspect conversation history
$messages = $chat->getMessages();

// Manually add context
$chat->addMessage('user', 'Previous context...');
```

### Streaming (SSE)

Stream AI responses to the browser in real time:

```php
$router->post('/api/chat/stream', function (Request $request) use ($gateway) {
    $message = $request->json('message');

    return Response::stream(function () use ($gateway, $message) {
        $gateway->streamChat(
            [['role' => 'user', 'content' => $message]],
            function (string $chunk) {
                echo "data: " . json_encode(['text' => $chunk]) . "\n\n";
                flush();
            },
        );
        echo "data: [DONE]\n\n";
    });
});
```
