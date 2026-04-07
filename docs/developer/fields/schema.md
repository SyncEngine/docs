# Schema Field (`type: schema`)

Schema editor built on grid with columns for key/type/required/readonly.

## Typical Props

- `source` (`data` supported)
- `choices`
- `customizable`
- `sortable`, `taggable`

## Example

```php
'schema' => [
    'label' => $this->trans('Schema'),
    'type' => 'schema',
    'sortable' => true,
]
```
