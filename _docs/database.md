---
title: "Database"
description: "Database connections, query builder, transactions, and migrations in Smallwork"
---

### Connecting

Configure in `config/database.php` or `.env`:

```php
// SQLite
$adapter = Connection::create([
    'driver'   => 'sqlite',
    'database' => 'storage/database.sqlite',
]);

// MySQL
$adapter = Connection::create([
    'driver'   => 'mysql',
    'host'     => '127.0.0.1',
    'port'     => 3306,
    'database' => 'myapp',
    'username' => 'root',
    'password' => '',
]);

// PostgreSQL
$adapter = Connection::create([
    'driver'   => 'pgsql',
    'host'     => '127.0.0.1',
    'port'     => 5432,
    'database' => 'myapp',
    'username' => 'postgres',
    'password' => '',
]);
```

### Query Builder

Fluent interface for building SQL queries:

```php
$db = Connection::create($config);
$qb = new QueryBuilder($db, 'users');

// SELECT
$users = $qb->select('id', 'name', 'email')
    ->where('active', '=', 1)
    ->orderBy('created_at', 'DESC')
    ->limit(10)
    ->offset(20)
    ->get();

// Single row
$user = (new QueryBuilder($db, 'users'))
    ->where('id', '=', 42)
    ->first();

// Count
$total = (new QueryBuilder($db, 'users'))
    ->where('active', '=', 1)
    ->count();

// INSERT (returns last insert ID)
$id = (new QueryBuilder($db, 'users'))->insert([
    'name'  => 'Alice',
    'email' => 'alice@example.com',
]);

// UPDATE (returns affected row count)
$affected = (new QueryBuilder($db, 'users'))
    ->where('id', '=', 42)
    ->update(['name' => 'Bob']);

// DELETE
$deleted = (new QueryBuilder($db, 'users'))
    ->where('active', '=', 0)
    ->delete();

// JOIN
$posts = (new QueryBuilder($db, 'posts'))
    ->select('posts.title', 'users.name')
    ->join('users', 'posts.user_id', '=', 'users.id')
    ->get();

// GROUP BY
$counts = (new QueryBuilder($db, 'orders'))
    ->select('status')
    ->groupBy('status')
    ->get();
```

### Transactions

```php
$db->beginTransaction();
try {
    (new QueryBuilder($db, 'accounts'))
        ->where('id', '=', 1)
        ->update(['balance' => 900]);

    (new QueryBuilder($db, 'accounts'))
        ->where('id', '=', 2)
        ->update(['balance' => 1100]);

    $db->commit();
} catch (\Throwable $e) {
    $db->rollback();
    throw $e;
}
```

### Migrations

Create migration files in `database/migrations/`:

```php
<?php
// database/migrations/2026_02_27_000001_create_users.php

use Smallwork\Database\Migration;
use Smallwork\Database\Schema;

return new class extends Migration {
    public function up(Schema $schema): void
    {
        $schema->create('users', function (Schema $table) {
            $table->id();
            $table->string('name');
            $table->string('email');
            $table->boolean('active')->nullable();
            $table->timestamps();
        });
    }

    public function down(Schema $schema): void
    {
        $schema->drop('users');
    }
};
```

Run migrations:

```bash
php smallwork migrate
php smallwork migrate --rollback
```

Or programmatically:

```php
$migrator = new Migrator($db, __DIR__ . '/database/migrations');
$count = $migrator->migrate();   // Run pending
$count = $migrator->rollback();  // Revert last batch
```
