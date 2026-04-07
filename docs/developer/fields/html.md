# HTML Field (`type: html`)

UI helper field that renders raw HTML (`dangerouslySetInnerHTML`).

Only use trusted content.

```php
'notice' => [
    'type' => 'html',
    'html' => '<div class="alert alert-info">Info</div>',
]
```

## Additional Examples

```php
'warning' => [
    'type' => 'html',
    'html' => '<p class="text-warning mb-0">This action cannot be undone.</p>',
]
```
