---
title: "CLI"
description: "Built-in CLI commands and creating custom commands in Smallwork"
---

The `smallwork` executable provides command-line tools:

```bash
# List all commands
php smallwork list

# Start development server
php smallwork serve
php smallwork serve --host=0.0.0.0 --port=3000

# Run database migrations
php smallwork migrate
php smallwork migrate --rollback

# Generate boilerplate
php smallwork make:controller User     # -> app/Controllers/UserController.php
php smallwork make:model Post          # -> app/Models/Post.php
php smallwork make:migration create_posts  # -> database/migrations/2026_02_27_120000_create_posts.php
```

### Custom Commands

Create commands by extending `Command`:

```php
<?php
namespace App\Console;

use Smallwork\Console\Command;

class SeedCommand extends Command
{
    public function getName(): string { return 'db:seed'; }
    public function getDescription(): string { return 'Seed the database'; }

    public function execute(array $args): int
    {
        // Seeding logic...
        echo "Database seeded.\n";
        return 0; // exit code
    }
}
```

Register in the CLI entry point:

```php
$cli->register('db:seed', new SeedCommand());
```
