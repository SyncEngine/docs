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

## Additional Examples

```php
'request_codec' => [
    'label' => $this->trans('Request codec'),
    'type' => 'codec',
    'direction' => 'encode',
    'compact' => true,
]
```

```php
'response_codec' => [
    'label' => $this->trans('Response codec'),
    'type' => 'codec',
    'filters' => ['module' => 'core'],
]
```
