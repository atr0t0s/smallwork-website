---
title: "Getting Started"
description: "Requirements, installation, project structure, and configuration for the Smallwork PHP AI framework"
---

## Requirements

- PHP 8.2+
- Composer
- cURL extension (for AI providers)
- Optional: Redis, Qdrant, PostgreSQL with pgvector

## Quick Start

```bash
# Install dependencies
composer install

# Copy environment config
cp .env.example .env

# Start development server
php smallwork serve

# Or use PHP directly
php -S localhost:8080 -t public
```

Visit `http://localhost:8080` to verify it's running.

## Project Structure

```
smallwork/
├── public/              # Web root (single entry point)
│   ├── index.php
│   └── .htaccess
├── config/
│   ├── app.php          # Application settings
│   ├── database.php     # Database connections
│   ├── auth.php         # JWT and RBAC config
│   ├── ai.php           # AI provider config
│   └── routes/
│       ├── api.php      # API route definitions
│       └── web.php      # Web route definitions
├── src/                 # Framework source code
│   ├── Core/            # Router, Request, Response, Container, Middleware
│   ├── Database/        # Query builder, migrations, adapters
│   ├── Auth/            # JWT, API keys, roles
│   ├── View/            # Template engine, HTMX helpers
│   ├── AI/              # Gateway, providers, chat, embeddings, search
│   ├── Console/         # CLI commands
│   └── Testing/         # Test helpers
├── app/                 # Your application code
│   ├── Controllers/
│   ├── Models/
│   ├── Middleware/
│   ├── Views/
│   └── Prompts/
├── database/
│   └── migrations/
├── storage/
│   ├── logs/
│   └── cache/
└── tests/
    ├── Unit/
    └── Integration/
```

## Configuration

Copy `.env.example` to `.env` and set your values:

```ini
APP_NAME=Smallwork
APP_ENV=local
APP_DEBUG=true

DB_DRIVER=sqlite
DB_DATABASE=storage/database.sqlite

REDIS_HOST=127.0.0.1
REDIS_PORT=6379

AI_PROVIDER=openai
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
GROK_API_KEY=xai-...
```

Access environment variables anywhere with the `env()` helper:

```php
$debug = env('APP_DEBUG', false);    // Casts "true"/"false" to booleans
$name  = env('APP_NAME', 'Default');
```
