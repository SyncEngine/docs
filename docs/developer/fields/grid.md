# Grid Field (`type: grid`)

Tabular editor for key/value rows and structured rows.

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
