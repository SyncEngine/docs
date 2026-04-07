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
    'fieldset' => $this->getAuthStepFields(),
]
```
