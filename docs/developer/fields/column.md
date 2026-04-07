# Column Field (`type: column`)

Selects a column type and renders column-specific config fields.

## Typical Props

- `columnTypes`
- `filters`, `compact`
- `view` (`full` or compact modal-style config)

## Example

```php
'column' => [
    'label' => $this->trans('Column type'),
    'type' => 'column',
]
```

## Additional Examples

```php
'column_type' => [
    'label' => $this->trans('Column transformer'),
    'type' => 'column',
    'compact' => true,
]
```

```php
'column_type' => [
    'type' => 'column',
    'filters' => ['module' => 'core'],
    'view' => 'full',
]
```
