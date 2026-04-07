# Params Field (`type: params`)

Parameter editor that can switch between grid mode and code mode with format support.

## Typical Props

- `columns`
- `customizable`
- `formats` (format choices for UI)
- `taggable`, `sortable`

## Example

```php
'headers' => [
    'label' => $this->trans('Request header'),
    'type' => 'params',
    'collapsed' => true,
    'taggable' => true,
]
```

## Additional Examples

```php
'query' => [
    'type' => 'params',
    'customizable' => true,
    'formats' => [ // Add format selection for UI
        'name' => 'format',
        'choices' => [
            'json' => 'JSON',
            'xml' => 'XML'
        ],
    ],
]
```

```php
'body' => [
    'type' => 'params',
    'columns' => [
        'key' => 'Key',
        'value' => 'Value'
    ],
    'sortable' => true,
    'taggable' => true,
]
```
