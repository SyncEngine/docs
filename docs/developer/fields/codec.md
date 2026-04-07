# Codec Field (`type: codec`)

Selects a codec type and optionally shows codec-specific config fields.

## Typical Props

- `columnTypes`
- `filters`, `compact`
- `view` (`full` or compact modal-style config)
- optional `direction` filtering in codec fields

## Example

```php
'decode' => [
    'label' => $this->trans('Decoder'),
    'type' => 'codec',
    'view' => 'full',
]
```
