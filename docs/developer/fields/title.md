# Title Field (`type: title`)

UI helper field for rendering a section title with optional help/description.

```php
'intro' => [
    'type' => 'title',
    'label' => $this->trans('Request settings'),
]
```

## Additional Examples

```php
'auth_title' => [
    'type' => 'title',
    'label' => $this->trans('Authorization'),
    'description' => $this->trans('These settings are used before each request.'),
]
```
