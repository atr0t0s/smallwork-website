---
layout: home
title: Home
---

<section class="hero">
  <div class="hero__inner">
    <h1 class="hero__title">Smallwork</h1>
    <p class="hero__tagline">A small footprint full-stack AI framework for PHP</p>
    <p class="hero__sub">Build AI-powered web applications with a unified multi-provider AI gateway, server-rendered templates, JSON APIs, vector search, and enterprise features — without the overhead of a large framework.</p>
    <div class="hero__actions">
      <a href="{{ '/docs/getting-started/' | relative_url }}" class="btn btn--primary">Get Started</a>
      <a href="https://github.com/atr0t0s/smallwork" class="btn btn--outline" target="_blank" rel="noopener">GitHub</a>
    </div>
    <div class="hero__code">
{% highlight bash %}
composer require atr0t0s/smallwork
cp .env.example .env
php smallwork serve
{% endhighlight %}
</div>
  </div>
</section>

<section class="features">
  <div class="features__inner">
    <div class="features__grid">
      <div class="feature">
        <div class="feature__icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2L2 7l10 5 10-5-10-5z"/><path d="M2 17l10 5 10-5"/><path d="M2 12l10 5 10-5"/></svg>
        </div>
        <h3 class="feature__title">Multi-Provider AI Gateway</h3>
        <p class="feature__desc">Unified interface across OpenAI, Anthropic, and Grok. Switch providers per request with zero code changes.</p>
      </div>
      <div class="feature">
        <div class="feature__icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="4 17 10 11 4 5"/><line x1="12" y1="19" x2="20" y2="19"/></svg>
        </div>
        <h3 class="feature__title">Templates + HTMX</h3>
        <p class="feature__desc">Server-rendered Blade-like templates with built-in HTMX helpers. Interactive UIs without writing JavaScript.</p>
      </div>
      <div class="feature">
        <div class="feature__icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
        </div>
        <h3 class="feature__title">Vector Search & RAG</h3>
        <p class="feature__desc">Embeddings, semantic search, and retrieval-augmented generation with Qdrant and pgvector support.</p>
      </div>
      <div class="feature">
        <div class="feature__icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2" ry="2"/><path d="M7 11V7a5 5 0 0110 0v4"/></svg>
        </div>
        <h3 class="feature__title">JWT Auth & RBAC</h3>
        <p class="feature__desc">JWT tokens, API key management, role-based access control with permission middleware out of the box.</p>
      </div>
      <div class="feature">
        <div class="feature__icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><ellipse cx="12" cy="5" rx="9" ry="3"/><path d="M21 12c0 1.66-4 3-9 3s-9-1.34-9-3"/><path d="M3 5v14c0 1.66 4 3 9 3s9-1.34 9-3V5"/></svg>
        </div>
        <h3 class="feature__title">Database & Query Builder</h3>
        <p class="feature__desc">Fluent query builder with SQLite, MySQL, and PostgreSQL. Migrations, transactions, and Redis support.</p>
      </div>
      <div class="feature">
        <div class="feature__icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 12h-4l-3 9L9 3l-3 9H2"/></svg>
        </div>
        <h3 class="feature__title">Built-in Middleware</h3>
        <p class="feature__desc">CORS, rate limiting, authentication, and AI-powered middleware for content moderation, intent classification, and summarization.</p>
      </div>
    </div>
  </div>
</section>

<section class="example">
  <div class="example__inner">
    <h2 class="example__title">Concise by design</h2>
    <p class="example__desc">Set up an AI-powered chat endpoint with streaming in a few lines:</p>
{% highlight php %}
$gateway = new Gateway(defaultProvider: 'openai');
$gateway->register('openai', new OpenAIProvider(
    baseUrl: 'https://api.openai.com/v1',
    apiKey: env('OPENAI_API_KEY'),
));

$router->post('/api/chat', function (Request $request) use ($gateway) {
    $chat = new Chat(gateway: $gateway, systemPrompt: 'You are a helpful assistant.');
    $response = $chat->send($request->json('message'));
    return Response::json($response);
});
{% endhighlight %}
  </div>
</section>
