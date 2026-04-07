# Repeater Field (`type: repeater`)

Generic repeatable block renderer for custom fieldsets.

## Typical Props

- `fieldset`/`fields`
- `actions` (`delete`, `disable`, `request`, `link`)
- `items` (optional custom item map)
- `sortable`, `inline`, `max`

## Example

```php
'authorization' => [
    'type' => 'repeater',
    'actions' => ['disable', 'delete'],
    'fieldset' => [
        // Repeater fields.
    ],
]
```

## Additional Examples

```php
'steps' => [
    'type' => 'repeater',
    'sortable' => true,
    'max' => 10,
    'fieldset' => [
        'name' => ['type' => 'text'],
        'enabled' => ['type' => 'toggle'],
    ],
]
```

```php
'requests' => [
    'type' => 'repeater',
    'actions' => [
        // Custom React request handler
        'run' => ['type' => 'request', 'props' => ['type' => 'connection', 'action' => 'send']],
        'disable',
        'delete',
    ],
]
```
