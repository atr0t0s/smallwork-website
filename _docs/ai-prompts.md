---
title: "Prompt Management"
description: "Versioned prompt templates with a template engine and version manager"
---

{% raw %}

Manage versioned prompt templates for A/B testing and iteration:

**Template files** (`app/Prompts/greeting.v1.prompt`):

```
Hello {{name}}, welcome to {{app}}! How can I help you today?
```

**Template Engine:**

```php
$engine = new TemplateEngine();

// Render inline template
$prompt = $engine->render(
    'Summarize this {{type}} in {{language}}: {{content}}',
    ['type' => 'article', 'language' => 'English', 'content' => $text],
);

// Render from file
$prompt = $engine->renderFile('app/Prompts/greeting.v1.prompt', [
    'name' => 'Alice',
    'app'  => 'Smallwork',
]);
// Throws RuntimeException if any {{placeholder}} is left unsubstituted
```

**Version Manager:**

```php
$versions = new VersionManager('app/Prompts');

// Discover available versions
$available = $versions->versions('greeting');  // [1, 2, 3]

// Get latest version content
$template = $versions->latest('greeting');

// Get specific version
$template = $versions->version('greeting', 2);

// Combine with template engine
$prompt = $engine->render($versions->latest('greeting'), ['name' => 'Bob', 'app' => 'MyApp']);
```

{% endraw %}
