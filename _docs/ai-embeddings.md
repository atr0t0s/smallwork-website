---
title: "Embeddings & Vector Search"
description: "Generate embeddings, build semantic search, and use vector stores for RAG pipelines"
---

### Embeddings

Generate vector embeddings from text:

```php
$embeddings = new Embeddings($gateway, maxChunkLength: 8000);

// Single text
$vectors = $embeddings->embed('The quick brown fox jumps over the lazy dog.');
// Returns array of float arrays (one per chunk if text is long)

// Batch
$vectors = $embeddings->embedBatch([
    'First document.',
    'Second document.',
    'Third document.',
]);
// Returns one embedding vector per input text

// Long text is automatically chunked at word boundaries
$vectors = $embeddings->embed($veryLongText);
// Returns one vector per chunk

// Use a specific provider
$vectors = $embeddings->embed($text, provider: 'openai', options: ['model' => 'text-embedding-3-large']);
```

### Semantic Search & RAG

Build retrieval-augmented generation (RAG) pipelines:

```php
$search = new SemanticSearch(
    gateway: $gateway,
    vectorStore: $qdrantAdapter,  // or $pgvectorAdapter
    collection: 'documents',
    provider: 'openai',
);

// Index documents
$search->index('doc-1', 'PHP is a server-side scripting language.', ['source' => 'wiki']);
$search->index('doc-2', 'Laravel is a PHP framework.', ['source' => 'docs']);

// Batch index
$search->indexBatch([
    ['id' => 'doc-3', 'text' => 'Composer manages PHP dependencies.', 'payload' => ['source' => 'docs']],
    ['id' => 'doc-4', 'text' => 'PHPUnit is a testing framework.', 'payload' => ['source' => 'docs']],
]);

// Search
$results = $search->search('What framework is used for PHP?', limit: 5);
// [['id' => 'doc-2', 'score' => 0.92, 'payload' => ['source' => 'docs', 'text' => '...']], ...]

// RAG: format search results as context for chat
$context = $search->formatRagContext('What frameworks exist?', $results);
$response = $gateway->chat([
    ['role' => 'system', 'content' => "Answer using this context:\n$context"],
    ['role' => 'user', 'content' => 'What frameworks exist for PHP?'],
]);
```

### Vector Stores

Two backends are supported. Both implement `VectorStoreInterface`:

**Qdrant** (recommended for production):

```php
$qdrant = new QdrantAdapter(
    host: 'http://localhost',
    port: 6333,
    apiKey: null, // optional
);

$qdrant->createCollection('documents', dimensions: 1536, distance: 'cosine');
$qdrant->upsert('documents', [
    ['id' => 'doc-1', 'vector' => [0.1, 0.2, ...], 'payload' => ['text' => '...']],
]);
$results = $qdrant->search('documents', $queryVector, limit: 10);
$qdrant->delete('documents', ['doc-1']);
```

**pgvector** (for teams already on PostgreSQL):

```php
$pgvector = new PgvectorAdapter($pdoAdapter);

$pgvector->createCollection('documents', dimensions: 1536, distance: 'cosine');
// Same interface as Qdrant: upsert(), search(), delete()
```

Distance metrics: `cosine`, `euclidean`, `dot`.
