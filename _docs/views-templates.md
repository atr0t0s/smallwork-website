---
title: "Views & Templates"
description: "Blade-like template engine with layouts, sections, and partials in Smallwork"
---

{% raw %}

### Blade-like Template Engine

Templates use `.sw.php` extension and live in `app/Views/`:

**Layout** (`app/Views/layouts/app.sw.php`):

```html
<!DOCTYPE html>
<html>
<head>
    <title>@yield('title') - My App</title>
    @yield('head')
</head>
<body>
    @yield('content')
    @yield('scripts')
</body>
</html>
```

**Page** (`app/Views/home.sw.php`):

```html
@extends('layouts.app')

@section('title')Home@endsection

@section('content')
    <h1>Welcome, {{ $name }}</h1>

    @if($isAdmin)
        <p>Admin panel: <a href="/admin">Go</a></p>
    @else
        <p>You are a regular user.</p>
    @endif

    <ul>
    @foreach($items as $item)
        <li>{{ $item }}</li>
    @endforeach
    </ul>

    @include('partials.footer')
@endsection
```

**Rendering:**

```php
$engine = new Engine(
    viewsPath: __DIR__ . '/app/Views',
    cachePath: __DIR__ . '/storage/cache',
);

// In a route handler
$html = $engine->render('home', ['name' => 'Alice', 'isAdmin' => true, 'items' => ['A', 'B']]);
return Response::html($html);

// Or use ViewResponse
$view = new ViewResponse($engine);
return $view->make('home', ['name' => 'Alice']);
```

**Template syntax reference:**

| Syntax | Description |
|--------|-------------|
| `{{ $var }}` | Escaped output (htmlspecialchars) |
| `{!! $var !!}` | Raw/unescaped output |
| `@if($cond)...@elseif($cond)...@else...@endif` | Conditionals |
| `@foreach($items as $item)...@endforeach` | Loop |
| `@for($i=0; $i<10; $i++)...@endfor` | For loop |
| `@while($cond)...@endwhile` | While loop |
| `@extends('layout')` | Inherit layout |
| `@section('name')...@endsection` | Define section |
| `@yield('name')` | Output section |
| `@include('partial')` | Include partial (dot notation for paths) |

{% endraw %}
