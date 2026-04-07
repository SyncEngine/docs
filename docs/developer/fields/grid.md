# Grid Field (`type: grid`)

Grid based editor for key/value rows and structured rows.

## Typical Props

- `columns`
- `sortable`, `removable`
- `indexed` (object mapping mode)
- `taggable`
- `nest`

## Example

```php
'params' => [
    'type' => 'grid',
    'sortable' => true,
    'taggable' => true,
    'columns' => [
        'find' => $this->trans('Find'),
        'replace' => $this->trans('Replace'),
    ],
]
```

## Additional Examples

```php
'headers' => [
    'type' => 'grid',
    'columns' => [
        'key' => $this->trans('Header'),
        'value' => $this->trans('Value'),
    ],
    'sortable' => true,
]
```

```php
'schema_map' => [
    'type' => 'grid',
    'indexed' => 'column',
    'columns' => [],
]
```
