---
title: "Input Validation"
description: "Validating request data with built-in rules in Smallwork"
---

```php
$validator = Validator::make($request->json(), [
    'name'     => 'required|string|min:2|max:100',
    'email'    => 'required|email',
    'age'      => 'required|numeric|min:18|max:120',
    'role'     => 'required|in:admin,user,editor',
    'tags'     => 'array',
]);

if ($validator->fails()) {
    return Response::json(['errors' => $validator->errors()], 422);
}
```

**Available rules:**

| Rule | Description |
|------|-------------|
| `required` | Field must be present and non-empty |
| `string` | Must be a string |
| `numeric` | Must be numeric |
| `email` | Must be a valid email format |
| `array` | Must be an array |
| `min:N` | Minimum length (string) or value (numeric) |
| `max:N` | Maximum length (string) or value (numeric) |
| `in:a,b,c` | Must be one of the listed values |
