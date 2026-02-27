---
title: "AI Gateway Setup"
description: "Configuring the unified multi-provider AI gateway for OpenAI, Anthropic, and Grok"
---

The AI Gateway provides a unified interface across OpenAI, Anthropic (Claude), and Grok:

```php
$gateway = new Gateway(defaultProvider: 'openai');

$gateway->register('openai', new OpenAIProvider(
    baseUrl: 'https://api.openai.com/v1',
    apiKey: env('OPENAI_API_KEY'),
    defaultModel: 'gpt-4o',
));

$gateway->register('anthropic', new AnthropicProvider(
    baseUrl: 'https://api.anthropic.com',
    apiKey: env('ANTHROPIC_API_KEY'),
    defaultModel: 'claude-sonnet-4-6',
));

$gateway->register('grok', new GrokProvider(
    baseUrl: 'https://api.x.ai/v1',
    apiKey: env('GROK_API_KEY'),
    defaultModel: 'grok-2',
));
```
